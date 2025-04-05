---
title: REST API Design
tags:
    - system-design
    - api
    - rest
---

A _Representational State Transfer Application Programming Interface_ is a set of rules and conventions that uses standard HTTP methods to perform operations on resources identified by URLs. REST APIs typically use JSON or XML for data transfer, are stateless (IE. each request contains all necessary information, and the server doesn't store any session data), and follow a client-server architecture that separates presentation and business logic/data storage concerns.

## Best practices
- **HTTP methods:** use correct methods, IE. retrieve data with GET, create data with POST, update data with PUT/PATCH, destroy data with DELETE
- **HTTP status codes:** EG. `200` (OK), `201` (Created), `400` (Bad Request), `401` (Unauthorized), `404` (Not Found), `500` (Server Error)
- **Resource naming:** use plural nouns for collections (EG. `/users`), and clear hierarchical relationships (EG. `/users/123/orders`)
- **Versioning:** include version in the URL, or use HTTP headers to handle evolution without breaking clients
- **Filtering, sorting, and pagination:** allow clients to request only what they need (EG. `/products?category=electronics&sort=price&page=2`)
- **Error messages:** include error codes, messages, and potentially links to documentation
- **[[HATEOAS]]:** include links to related resources in responses to help clients navigate the API
- **Content negotiation:** support multiple formats (JSON, XML, etc.) via Accept headers
- **Idempotence:** repeated identical requests should have the same effect as a single request
- **Security:** implement authentication, authorisation, HTTPS, rate limiting, and/or input validation
- **Documentation:** provide clear, complete documentation providing examples and API references using EG. Swagger/OpenAPI
- **Response:** standardise response formats for predictability

## Pagination
Pagination is a technique for dividing large datasets into smaller, more manageable chunks when returning results from an API or displaying content on a website. It improves both performance and user experience by limiting the amount of data transferred in a single request.

### Offset-based pagination
`GET /posts%limit=25&offset=100`

_Offset-based pagination_ can be compared to pages in a book. The above example can be thought of as saying "show me 25 posts, starting after the first 100". This is a both conceptually and technically straightforward approach, but has a significant drawback in that a database would need to scan and skip over `n` rows before returning the next result.

### Cursor-based pagination
`GET /posts?limit=25&cursor=jdfui1u8913==`

_Cursor-based pagination_ keeps track of the current position using a pointer/cursor, which is typically an encoded value representing the last seen item. This approach gains efficiency over offset-based pagination because of its use of indexed fields to directly filter to the starting point, rather than counting offsets.

```python
from flask import Flask, request, jsonify
import base64
import json

app = Flask(__name__)

# Sample data - in a real app, this would be in a database
messages = [
    {"id": 1, "text": "First message", "timestamp": "2025-04-05T09:00:00Z"},
    {"id": 2, "text": "Second message", "timestamp": "2025-04-05T09:10:00Z"},
    {"id": 3, "text": "Third message", "timestamp": "2025-04-05T09:20:00Z"},
    # ... more messages
    {"id": 50, "text": "Fiftieth message", "timestamp": "2025-04-05T14:30:00Z"}
]

@app.route('/api/messages', methods=['GET'])
def get_messages():
    # Get pagination parameters
    limit = int(request.args.get('limit', 10))
    cursor = request.args.get('cursor')
    
    # Find starting position based on cursor
    start_idx = 0
    if cursor:
        try:
            # Decode cursor from base64
            cursor_data = json.loads(base64.b64decode(cursor).decode('utf-8'))
            cursor_id = cursor_data.get('id')
            
            # Find the position after the cursor
            for idx, message in enumerate(messages):
                if message['id'] == cursor_id:
                    start_idx = idx + 1
                    break
        except Exception as e:
            return jsonify({"error": "Invalid cursor"}), 400
    
    # Get paginated results
    end_idx = min(start_idx + limit, len(messages))
    paginated_messages = messages[start_idx:end_idx]
    
    # Generate next cursor if there are more results
    has_more = end_idx < len(messages)
    next_cursor = None
    if has_more and paginated_messages:
        last_id = paginated_messages[-1]['id']
        cursor_data = json.dumps({"id": last_id})
        next_cursor = base64.b64encode(cursor_data.encode('utf-8')).decode('utf-8')
    
    return jsonify({
        "data": paginated_messages,
        "pagination": {
            "next_cursor": next_cursor,
            "has_more": has_more
        }
    })

if __name__ == '__main__':
    app.run(debug=True)
```

### Keyset-based pagination
`GET /api/messages?limit=50&before=2024-03-15T00:00:00Z`

_Keyset-based pagination_ uses the values of ordered columns (like ID or timestamp) to filter subsequent database queries, rather than using offset or page numbers. This technique is particularly powerful when used in conjunction with indexed database columns.

**Description:**
1. A query includes a `WHERE` clause that filters records based on the key values of the last record from the previous page
2. Records are sorted by the same key(s) used in the filtering
3. The client sends the values of these keys to get the next page

**Differences from cursor-based pagination:**

1. **Implementation:**
    - Keyset pagination directly uses actual database column values in queries
    - Cursor-based pagination typically encodes/encrypts position information into an opaque string
2. **Transparency:**
    - Keyset pagination uses clear, visible parameters (e.g., `?after_id=1234&after_date=2025-01-01`)
    - Cursor-based pagination uses opaque tokens (e.g., `?cursor=eyJpZCI6MTIzNH0=`)
3. **Flexibility:**
    - Keyset pagination allows clients to jump to specific points in the dataset if they know key values
    - Cursor-based pagination typically only allows sequential navigation

Both approaches solve the same core problems with offset pagination (performance issues and inconsistencies when data changes), but keyset pagination exposes the implementation details while cursor-based pagination abstracts them away.

```python
from flask import Flask, request, jsonify
import sqlite3
from datetime import datetime

app = Flask(__name__)

# Set up a sample database (in a real app, you'd use proper DB configuration)
def get_db():
    conn = sqlite3.connect('messages.db')
    conn.row_factory = sqlite3.Row
    return conn

# Initialize database with sample data
def init_db():
    conn = get_db()
    cursor = conn.cursor()
    cursor.execute('''CREATE TABLE IF NOT EXISTS messages 
                     (id INTEGER PRIMARY KEY, text TEXT, created_at TEXT)''')
    
    # Insert sample data if table is empty
    cursor.execute("SELECT COUNT(*) FROM messages")
    if cursor.fetchone()[0] == 0:
        for i in range(1, 51):
            # Create timestamps with increasing values
            timestamp = datetime.fromisoformat(f"2025-04-05T09:{i:02d}:00")
            cursor.execute("INSERT INTO messages (text, created_at) VALUES (?, ?)",
                          (f"Message {i}", timestamp.isoformat()))
    conn.commit()
    conn.close()

@app.route('/api/messages', methods=['GET'])
def get_messages():
    # Get pagination parameters
    limit = int(request.args.get('limit', 10))
    after_id = request.args.get('after_id')
    after_date = request.args.get('after_date')
    
    conn = get_db()
    cursor = conn.cursor()
    
    # Build query based on keyset pagination parameters
    query = "SELECT id, text, created_at FROM messages"
    params = []
    
    if after_id and after_date:
        # Keyset condition using composite key (date, id)
        query += " WHERE (created_at > ?) OR (created_at = ? AND id > ?)"
        params = [after_date, after_date, after_id]
    
    # Add ordering and limit
    query += " ORDER BY created_at ASC, id ASC LIMIT ?"
    params.append(limit)
    
    # Execute query
    cursor.execute(query, params)
    messages = [dict(row) for row in cursor.fetchall()]
    
    # Check if there are more results
    has_more = len(messages) == limit
    
    # Generate pagination info for next page
    next_params = {}
    if has_more and messages:
        next_params = {
            'after_id': messages[-1]['id'],
            'after_date': messages[-1]['created_at']
        }
    
    return jsonify({
        "data": messages,
        "pagination": {
            "has_more": has_more,
            "next_params": next_params
        }
    })

if __name__ == '__main__':
    init_db()
    app.run(debug=True)
```