# postman-api-testing-portfolio
This portfolio includes 10 tests covering basic functionality to advanced automation. Each builds on Postman features like pre-request scripts, environment/collection variables, and schema validation.

Test #	Name	Key features demonstrated
1	Basic GET Request	Status 200, response time <2s, non-empty image data learning.postman
2	Query Parameters	Limit/size params, secure HTTPS URLs, range validation (1-10 images)
3	Environment Variables	Extract & store breed_id from live data for chaining
4	Request Chaining	Use {{breed_id}} to fetch breed-specific images
5	Negative Test	Invalid breed_id handled gracefully (empty array, status 200)
6	Data Type Validation	ID string, HTTPS URL, positive width/height numbers
7	Collection Variables	Configurable {{test_breed_id}} & {{default_limit}}
8	Schema Validation	JSON schema match for response structure
9	Pre-request Script	Dynamic random_limit, timestamp, custom headers
10	Comprehensive Suite	HTTP basics, structure, data quality in one run 
