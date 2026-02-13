# postman-api-testing-portfolio
This portfolio includes 10 tests covering basic functionality to advanced automation. Each builds on Postman features like pre-request scripts, environment/collection variables, and schema validation.

## How to Use
1. Import `Cat-API-Testing-Portfolio.postman_collection.json` into Postman
2. Import `Cat-API-Development.postman_environment.json` into Postman
3. Select the environment from dropdown
4. Run individual tests or entire collection

## Test 1: Basic GET Request – Random Cat Image

**Purpose**  
Confirms that the API successfully delivers cat images when requested. This test checks if the chosen image‑retrieval function works, responds quickly, and provides valid data.

**How It Works**  
The test sends a simple “GET” request to the API’s /images/search endpoint, which is designed to return one or more random cat images.

**What’s Checked**

- The API responds successfully (status code 200).
    
- The server replies within 2 seconds (performance check).
    
- The response actually includes image data (not an empty result).
    
**Why It Matters**  
This test ensures that the most basic feature of the API — returning cat images — works reliably.

## Test 2: Query Parameters – Multiple Cat Images

**Purpose**  
Checks whether the API correctly handles query parameters (like how many images to return) and whether each image is securely accessible.

**How It Works**  
The test sends a “GET” request asking for 5 small‑sized cat images (/images/search?limit=5&size=small). The API should ideally return 5 results, but it sometimes returns between 1 and 10 instead, so the test captures both outcomes.

**What’s Checked**

- The number of images returned matches the expected range (1–10).
    
- Each image includes the necessary details (ID, URL, width, height).
    
- Every image link uses a secure “https://” address.
    
- Logs the difference between the requested and actual number of images for debugging.
    
**Why It Matters**  
This test shows how the system reacts to query parameters and whether it consistently delivers valid, secure data.

## Test 3: Environment Variables – Breed Data Extraction

**Purpose**  
Prepares later tests by taking real cat breed data from the API and saving one breed’s ID into a reusable variable.

**How It Works**

- Sends a GET request to {{base_url}}/{{api_version}}/images/search, which returns cat images along with breed information in the same response.
    
- Confirms the API responds successfully (status 200) and that the list of returned items is not empty.
    
- Checks that each item has an ID and logs any cases where the breed name is missing (this can happen with incomplete data).
    
- Saves the ID of the first breed into an environment variable called breed_id, so other tests can automatically reuse it without hard‑coding values.
    
**Expected Results**

- Pass: The API returns status 200, there is at least one item in the list, and breed_id is stored for later tests.
    
- Note: Some breeds may not have a name filled in; these are just logged for information and do not cause the test to fail.
    
**What’s Covered**

- Using environment variables to share data between tests.
    
- Handling real‑world, sometimes incomplete data without breaking the test.
    
- Setting up “request chaining” so follow‑up tests can use live API values instead of fixed test data.

## Test 4: Request Chaining – Breed‑Specific Images

**Purpose**  
Uses the breed ID saved by Test 3 to request images of that specific cat breed, demonstrating how tests can chain together using real data.

**How It Works**  
Sends a GET request to /images/search?breed_ids={{breed_id}}&limit=3, where {{breed_id}} is the value automatically pulled from the environment variable set by Test 3.

**What’s Checked**

- The API responds successfully (status 200).
    
- The number of images returned is acceptable (0 or more is fine, as rare breeds may have no images).
    
- If images are returned, each one’s breed information includes an ID that matches the requested breed_id, plus a breed name.
    
**Why It Matters**  
This test shows how to build realistic workflows where one API call’s output (like a breed ID) feeds directly into the next, using environment variables to make tests more dynamic. It also handles real‑world edge cases, like breeds with no available images, without breaking the entire test suite.

## Test 5: Negative Test - Invalid Breed ID

**Purpose**  
Verifies that the API handles invalid breed IDs gracefully without crashing or returning errors, maintaining stability even when given incorrect data.

**How It Works**

- Sends a request with a deliberately invalid breed ID: GET /images/search?breed_ids=INVALID999
    
- The API accepts the invalid input without crashing
    
- Returns either an empty array \[\] or images with no breed information attached
    
- Validates that the response remains properly formatted JSON despite the bad input
    

**What's Checked**

- Status code is 200 - API responds successfully (doesn't treat it as an error)
    
- Graceful handling - Returns either zero results or images without breed data (breeds array is empty)
    
- Response structure - JSON array format is preserved even with invalid input
    
**Why It Matters**  
This test demonstrates that the API prioritizes stability over strict validation. Instead of crashing or returning error codes when users make mistakes, it returns empty results gracefully.

## Test 6: Data Type Validation

**Purpose**  
Ensures every piece of data returned by the API has the correct format and type - text fields contain text, numbers are actual numbers, and URLs are secure and properly formatted.

**How It Works**

- Requests exactly one image: GET /images/search?limit=1
    
- Examines the first image's core properties:
    
- id: Must be a non-empty text string
    
- url: Must start with https:// (secure URL)
    
- width & height: Must be positive numbers (not zero or negative)
    
- Verifies the server sends proper JSON formatting via the Content-Type header
    

**What's Checked**

- Status code 200 - API call succeeds
    
- ID validation - Non-empty string
    
- URL validation - Secure HTTPS format
    
- Dimension validation - Width/height are positive numbers
    
- JSON header - Server promises JSON content
    
**Why It Matters**  
Data type validation catches common API bugs like numbers stored as text strings or missing decimal points, which break client applications. This test ensures the API delivers clean data that developers can rely on.

## Test 7: Collection Variables in Action

**Purpose**  
Demonstrates using collection variables ****(shared settings for the entire test suite) to make tests configurable, reusable, and easy to maintain across different environments or projects.

**How It Works**

- Requests cat images filtered by breed using collection settings:  
    GET /images/search?breed_ids={{test_breed_id}}&limit={{default_limit}}
    
- Reads configuration values directly from collection variables:  
    {{test_breed_id}} (e.g., "beng" for Bengal cats)  
    {{default_limit}} (e.g., 10 images max)
    
- Logs the actual values used for debugging and verification
    

**What's Checked**

- Status 200 - Request succeeds
    
- Limit respected - Number of images ≤ default_limit
    
- Breed filtering - All images with breed data match test_breed_id
    
- Variable resolution - Collection settings load correctly (logged to console)
    
**Why It Matters**  
Collection variables act like a configuration dashboard for the entire test suite. This eliminates hard-coded values, makes tests portable across teams/environments, and shows professional test maintenance practices.

## Test 8: Schema Validation

**Purpose**  
Confirms that the API response follows a strict structural blueprint by validating it against a JSON Schema—a formal specification that defines exactly what a valid response should look like.

**How It Works**

- Request: GET /images/search?limit=1
    
- Defines a JSON Schema that specifies:  
    Response must be an array  
    Each image must contain: id, url, width, height  
    Field requirements: id and url are text, width and height are numbers ≥ 1, URLs start with https://
    
- Runs the schema validator to check the entire response at once
    
- If validation fails, logs detailed error messages to identify the exact problem
    

**What's Checked**

- Status code: Server responds with 200 (success)
    
- Schema match: Response structure perfectly matches the defined blueprint
    
- Required fields: Each image object contains all mandatory fields (id, url, width, height)
    
- Error diagnostics: If validation fails, console shows which field broke the rules and why
    

**Why It Matters**

- Contract testing: Treats the schema as a formal agreement between the API and client—if the API changes unexpectedly, this test catches it immediately.
    
- Faster debugging: Instead of manually checking each field, one test validates everything. When something breaks, one gets a specific error message (e.g., "field width expected number but got string") rather than vague failures.
    
- Reusable quality standard: The same schema can be used across multiple tests or endpoints, ensuring consistency. When updated, all related tests automatically use the new rules.
    
- Production confidence: Guarantees that every response has the exact structure one’s application expects, preventing runtime errors caused by missing or incorrectly formatted data.
    
**Notes**  
You may see a tv4 deprecation warning in the console—this is cosmetic and doesn't affect test functionality. The test performs two layers of validation: first against the full schema, then a direct check that required fields exist.

## Test 8: Schema Validation

**Purpose**  
Confirms that the API response follows a strict structural blueprint by validating it against a JSON Schema—a formal specification that defines exactly what a valid response should look like.

**How It Works**

- Request: GET /images/search?limit=1
    
- Defines a JSON Schema that specifies:  
    Response must be an array  
    Each image must contain: id, url, width, height  
    Field requirements: id and url are text, width and height are numbers ≥ 1, URLs start with https://
    
- Runs the schema validator to check the entire response at once
    
- If validation fails, logs detailed error messages to identify the exact problem
    

**What's Checked**

- Status code: Server responds with 200 (success)
    
- Schema match: Response structure perfectly matches the defined blueprint
    
- Required fields: Each image object contains all mandatory fields (id, url, width, height)
    
- Error diagnostics: If validation fails, console shows which field broke the rules and why
    

**Why It Matters**

- Contract testing: Treats the schema as a formal agreement between the API and client—if the API changes unexpectedly, this test catches it immediately.
    
- Faster debugging: Instead of manually checking each field, one test validates everything. When something breaks, one gets a specific error message (e.g., "field width expected number but got string") rather than vague failures.
    
- Reusable quality standard: The same schema can be used across multiple tests or endpoints, ensuring consistency. When updated, all related tests automatically use the new rules.
    
- Production confidence: Guarantees that every response has the exact structure one’s application expects, preventing runtime errors caused by missing or incorrectly formatted data.
    

**Notes**  
You may see a tv4 deprecation warning in the console—this is cosmetic and doesn't affect test functionality. The test performs two layers of validation: first against the full schema, then a direct check that required fields exist.

## Test 9 - Pre-request Script Example

**Purpose**  
Demonstrates full test automation by generating dynamic test data before the request is sent, ensuring each run is unique and realistic without manual setup.

**How It Works**

- Before request (pre-request script):  
    Generates a random limit (1-10) and saves it to the environment as random_limit  
    Creates an ISO timestamp and saves as test_timestamp  
    Adds a custom header X-Test-Run-ID (e.g., test-1700000000000)  
    Logs values to console
    
- Request: GET /images/search?limit={{random_limit}} (uses the dynamic limit)
    
- After response (test script):  
    Verifies status 200  
    Confirms API returns exactly 10 images (ignores the limit parameter)  
    Checks custom header was sent  
    Logs: "Sent limit=X, API ignored it and sent 10 images"
    

**What's Checked**

- Status code: 200 (success)
    
- Response size: Exactly 10 images, regardless of random limit sent (documents’ observed API behavior)
    
- Custom header: X-Test-Run-ID is present on the request
    
- Dynamic data: Reads random_limit from environment to confirm setup worked
    

**Why It Matters**

- End-to-end automation: Handles setup (random data, headers, timestamps), execution, and validation automatically—no manual changes needed between runs.
    
- Real-world simulation: Mimics production variability (e.g., random limits, tracking headers) to catch issues like parameter ignoring early.
    
- Debugging insights: Console logs show exactly what data was generated and sent vs. what the API returned, speeding up troubleshooting.
    
- Re-usable patterns: Pre-request scripts can be shared across tests, reducing duplication and enabling data chaining (e.g., one test's output feeds another's input).
    

**Notes**  
The API currently ignores limit without an API key, always returning 10 images—this test explicitly documents and verifies that behavior. Run it multiple times to see different random_limit values (1-10) in action.

## Test 10: Comprehensive Test Suite

**Purpose**  
Serves as a complete health check (smoke/regression test) for the API endpoint, validating HTTP basics, response structure, and data quality in one organized run to confirm production readiness.

**How It Works**

- Pre-request: Records start time (suite_start_time) and logs "COMPREHENSIVE TEST SUITE STARTED"
    
- Request: GET /images/search?limit=5
    
- Post-response:  
    Parses JSON response and calculates total suite time  
    Runs \~10 grouped tests across 3 sections (HTTP, structure, data)  
    Checks each image individually where needed (e.g., required fields, URLs)
    

**What's Checked**

- HTTP basics (3 checks): Status 200, response <3000ms, Content-Type includes application/json
    
- Response structure (3 checks): Array format, non-empty, length ≤10 (even with limit=5)
    
- Data quality (4+ checks): All images have id, url, width, height; IDs unique; every URL starts with https:// and includes .com
    

**Why It Matters**

- Quick confidence: One click runs multiple validations—ideal for daily regression or CI/CD pipelines to catch regressions fast.
    
- Organized coverage: Grouped tests make failures easy to pinpoint (e.g., "Image 2 missing height" vs. vague errors).
    
- Performance insight: Tracks suite and response times, helping identify slowdowns before they impact users.
    
- Realistic limits: Validates practical behavior (≤10 images without API key), matching docs and preventing surprises in production.



