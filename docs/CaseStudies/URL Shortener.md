# URL Shortener System Design

### Overview
Design a scalable URL shortening service.

# URL Shortener — Q&A

A Q&A style reference for the URL Shortener system design.

---

## Q1: What is a URL Shortener?

**Answer:**  
A URL Shortener is a service that converts a long URL into a short, unique, and easily shareable URL.  
Example:
```
Original: https://www.example.com/articles/2025/long-article-title  
Short: https://short.ly/abc123
```
---

## Q2: What are the key features of a URL Shortener?

**Answer:**  
1. Generate unique short URLs.  
2. Redirect users from short URL to the original long URL.  
3. Track usage analytics (optional).  
4. Handle collisions gracefully.  
5. Support custom aliases (optional).  

---

## Q3: What are the main components of a URL Shortener system?

**Answer:**  
1. **API Server** — Accepts requests to create or retrieve URLs.  
2. **Database** — Stores the mapping between short and long URLs.  
3. **Cache** — (Optional) For faster redirection using systems like Redis.  
4. **Hashing / Encoding Logic** — Converts long URL to short key.  
5. **Frontend / Dashboard** — Optional UI for users.  

---

## Q4: How do you generate a unique short URL key?

**Answer:**  
- **Hashing the long URL** (e.g., MD5, SHA) → Base62 encoded.  
- **Random string generation** (6-8 characters).  
- **Incremental ID** → encode to Base62.  

**Considerations:**  
- Avoid collisions.  
- Make the key short and URL-friendly.  
- Optionally allow custom aliases.  

---

## Q5: How to handle collisions?

**Answer:**  
- Check the database if the generated key already exists.  
- If yes, regenerate a new key (loop until unique).  
- Use probabilistic approaches (like using longer keys for high volume).  

---

## Q6: What databases can we use?

**Answer:**  
- **Relational DBs:** MySQL, PostgreSQL — easy mapping with unique constraints.  
- **NoSQL:** DynamoDB, MongoDB — good for scalability.  
- **In-memory cache:** Redis or Memcached — for fast redirection.  

---

## Q7: How to scale a URL Shortener?

**Answer:**  
1. **Database Sharding** — Distribute the mappings across multiple DBs.  
2. **Caching** — Use Redis to store hot keys.  
3. **Load Balancing** — Multiple API servers.  
4. **CDN for redirect** — Optional for global low-latency redirection.  

---

## Q8: How to ensure high availability?

**Answer:**  
- Use replicated databases.  
- Multi-zone deployment of API servers.  
- Backup and restore strategies for URL mappings.  

---

## Q9: What security considerations exist?

**Answer:**  
- Validate long URLs to prevent phishing/malware.  
- Rate-limit API requests.  
- Use HTTPS for all short URLs.  
- Optional: authentication for creating custom URLs.  

---

## Q10: Optional enhancements

**Answer:**  
- Expiration time for short URLs.  
- Analytics dashboard: clicks, geography, referrer.  
- Custom vanity URLs.  
- QR code generation.  


### Diagram
![URL Shortener Diagram](../Diagrams/url_shortener.png)