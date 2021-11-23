# Errors

<aside class="notice">
DynamiCore uses conventional HTTP response codes to indicate the success or failure of an API request. In general: Codes in the 2xx range indicate success. Codes in the 4xx range indicate an error that failed given the information provided (e.g., a required parameter was omitted, a charge failed, etc.). Codes in the 5xx range indicate an error with Ictineo servers (these are rare).
</aside>

HTTP STATUS CODE SUMMARY

| Error Code | Meaning                                                                                |
| ---------- | -------------------------------------------------------------------------------------- |
| 401        | Unauthorized                                                                           |
| 403        | Forbidden                                                                              |
| 404        | Not Found - The resource you are trying to access cannot be found                      |
| 409        | Conflict - A conflict occurred on the server while processing your request             |
| 500        | Internal Server Error - A serious error occurred on the server, please try again later |
