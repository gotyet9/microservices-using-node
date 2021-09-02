# Microservices
A single microservice contains
- routing
- middlewares
- business logic
- database access

to implement one feature of the application.

## Database-Per-Service pattern:

- We want each service to run independently of other services
    - If anything goes wrong, all our services will be down
    - Scaling single database will be really challenging
    - In case Service A reaches to Database B for some reason, then when Database B fails, even then Service A crashes. So we loose both Service A and B
    - Uptime is eventually increased
- Database schema/structure might change unexpectedly
    - Database B schema change breaks Service A (if that uses it)
- Some services might function more efficiently with different types of DB’s (sql vs nosql)

## Communication strategies between services:
- Sync 
    - Uses direct requests between services
- Async
    - Common Event bus - automatically handles incoming events and route them to different services who might be able to handle the event
- Conceptually easy to understand
- Introduces a dependency between services
- If any inter-service fails, the overall request fails
- Can easily introduce a web of different requests/events being emitted
- Async Pattern 2
    - An extra service that has zero dependencies on other services, instead just depends on event bus which sends it events along with data if any
    - This extra service will be extremely fast as it will only connect with DB that contains data such as IDs or say primary keys
    - Data duplication issue, paying for extra storage 
    - Harder to understand
