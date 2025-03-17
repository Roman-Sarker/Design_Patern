Here is a detailed list of **REST API HTTP status codes** and their meanings:  

---

### **1xx: Informational Responses**
| **Status Code** | **Meaning** | **Description** |
|--------------|------------|--------------|
| **100** Continue | Request received, continue processing | Client should continue sending request data. |
| **101** Switching Protocols | Protocol change request accepted | Server is switching to a different protocol. |

---

### **2xx: Success Responses**
| **Status Code** | **Meaning** | **Description** |
|--------------|------------|--------------|
| **200** OK | Request successful | The request was processed successfully. |
| **201** Created | Resource created successfully | A new resource has been created successfully. |
| **202** Accepted | Request accepted for processing | Request is received but still processing. |
| **204** No Content | Successful, but no data returned | Request was processed, but no response body is sent. |

---

### **3xx: Redirection Responses**
| **Status Code** | **Meaning** | **Description** |
|--------------|------------|--------------|
| **301** Moved Permanently | Resource moved to a new URL | The resource has been permanently relocated. |
| **302** Found | Resource temporarily moved | The resource is temporarily available at a different URL. |
| **304** Not Modified | Resource not changed | The cached version is still valid; no need to resend data. |

---

### **4xx: Client Error Responses**
| **Status Code** | **Meaning** | **Description** |
|--------------|------------|--------------|
| **400** Bad Request | Invalid request | The request is malformed or has incorrect syntax. |
| **401** Unauthorized | Authentication required | User needs to authenticate before accessing the resource. |
| **403** Forbidden | Access denied | User is authenticated but does not have permission. |
| **404** Not Found | Resource not found | The requested resource does not exist. |
| **405** Method Not Allowed | HTTP method not supported | The requested HTTP method (GET, POST, etc.) is not allowed. |
| **408** Request Timeout | Request took too long | The server timed out waiting for the request. |
| **409** Conflict | Conflict in request data | The request conflicts with existing data (e.g., duplicate entry). |
| **415** Unsupported Media Type | Content-Type not supported | The request format is not supported by the server. |

---

### **5xx: Server Error Responses**
| **Status Code** | **Meaning** | **Description** |
|--------------|------------|--------------|
| **500** Internal Server Error | Unexpected error | A general error occurred on the server. |
| **502** Bad Gateway | Invalid response from upstream | Server received an invalid response from another server. |
| **503** Service Unavailable | Server temporarily overloaded | The server is down for maintenance or is overloaded. |
| **504** Gateway Timeout | Upstream server not responding | The server is waiting for a response from another service but timed out. |

---

### **🔹 Common Status Codes Used in REST APIs**
| **Action** | **Status Code** | **Example** |
|------------|---------------|------------|
| Retrieve data | **200 OK** | GET `/users/1` |
| Create new resource | **201 Created** | POST `/users` |
| Update resource | **200 OK** or **204 No Content** | PUT `/users/1` |
| Delete resource | **204 No Content** | DELETE `/users/1` |
| Invalid request | **400 Bad Request** | Missing parameters in request |
| Unauthorized | **401 Unauthorized** | Access token required |
| Resource not found | **404 Not Found** | GET `/non-existent-page` |
| Internal server error | **500 Internal Server Error** | Database connection failure |

Would you like examples for specific use cases? 🚀
