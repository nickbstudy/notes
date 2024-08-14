**200 - Success**

- `200` OK - Will usually include response 
- `201` Created - includes copy of item
- `204` OK, no response

**400 - Client mistake**
- `400` - Bad Request, wrong args/etc
- `401` - Unauthorized - no credentials provided
- `403` - Forbidden - you don't have access
- `404` - Not Found
- `409` - Conflict - somebody else editing

**500 - Server error**
Just use `500`, don't be too specific.