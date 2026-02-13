# postman-api-testing-portfolio
This portfolio includes 10 tests covering basic functionality to advanced automation. Each builds on Postman features like pre-request scripts, environment/collection variables, and schema validation.

## How to Use
1. Import `Cat-API-Testing-Portfolio.postman_collection.json` into Postman
2. Import `Cat-API-Development.postman_environment.json` into Postman
3. Select the environment from dropdown
4. Run individual tests or entire collection

## Postman Cat API Tests

These 10 tests demonstrate key Postman features for testing The Cat API, from basic requests to advanced chaining and validation. They're designed for a junior QA portfolio, showing practical skills like data sharing and error handling. [perplexity](https://www.perplexity.ai/search/9950b7ad-7135-4d00-959f-1e450b3c1dcf)

## Test 1: Basic GET Request

**Purpose**  
Confirms the API delivers cat images reliably. Checks if the image-retrieval function works quickly with valid data. [perplexity](https://www.perplexity.ai/search/f14b29d1-5967-41b3-9dba-030d6cfa3b2e)

**How It Works**  
Sends a GET request to `/images/search`. [perplexity](https://www.perplexity.ai/search/a574436f-7bb9-4b40-a000-8069048b2612)

**What's Checked**  
- Status code 200.  
- Response under 2 seconds.  
- Includes image data (not empty). [perplexity](https://www.perplexity.ai/search/f14b29d1-5967-41b3-9dba-030d6cfa3b2e)

**Why It Matters**  
Ensures the core feature—returning cat images—works every time. [perplexity](https://www.perplexity.ai/search/a574436f-7bb9-4b40-a000-8069048b2612)

## Test 2: Query Parameters

**Purpose**  
Tests handling of parameters like image count and verifies secure access. [perplexity](https://www.perplexity.ai/search/c63f209c-100a-4c22-aa80-af7310155858)

**How It Works**  
GET `/images/search?limit=5&size=small`. Expects 1-10 images. [perplexity](https://www.perplexity.ai/search/9950b7ad-7135-4d00-959f-1e450b3c1dcf)

**What's Checked**  
- Images in 1–10 range.  
- Each has ID, URL, width, height.  
- URLs use https://.  
- Logs requested vs. actual count. [perplexity](https://www.perplexity.ai/search/c63f209c-100a-4c22-aa80-af7310155858)

**Why It Matters**  
Shows consistent, secure data with parameters. [perplexity](https://www.perplexity.ai/search/c63f209c-100a-4c22-aa80-af7310155858)

## Test 3: Environment Variables

**Purpose**  
Extracts breed data and saves an ID for reuse in later tests. [perplexity](https://www.perplexity.ai/search/13c387da-09a5-4177-bdef-9498e14c7c84)

**How It Works**  
GET `{{base_url}}/{{api_version}}/images/search`. Stores first breed ID as `breed_id`. [perplexity](https://www.perplexity.ai/search/c9c6089e-64c9-4864-911f-72313e34f18e)

**Expected Results**  
- Status 200.  
- At least one item.  
- `breed_id` saved. Logs missing breed names. [perplexity](https://www.perplexity.ai/search/13c387da-09a5-4177-bdef-9498e14c7c84)

**What's Covered**  
Environment variables, incomplete data handling, request chaining setup. [perplexity](https://www.perplexity.ai/search/c9c6089e-64c9-4864-911f-72313e34f18e)

## Test 4: Request Chaining

**Purpose**  
Uses `breed_id` from Test 3 to fetch breed-specific images. [perplexity](https://www.perplexity.ai/search/13c387da-09a5-4177-bdef-9498e14c7c84)

**How It Works**  
GET `/images/search?breed_ids={{breed_id}}&limit=3`. [perplexity](https://www.perplexity.ai/search/264f7d5e-7211-447d-a880-b7f0ceb1aa68)

**What's Checked**  
- Status 200.  
- 0+ images (handles rare breeds).  
- Matching breed ID and name if present. [perplexity](https://www.perplexity.ai/search/264f7d5e-7211-447d-a880-b7f0ceb1aa68)

**Why It Matters**  
Builds dynamic workflows with real API data. [perplexity](https://www.perplexity.ai/search/264f7d5e-7211-447d-a880-b7f0ceb1aa68)

## Test 5: Invalid Breed ID

**Purpose**  
Ensures graceful handling of bad input without crashes. [perplexity](https://www.perplexity.ai/search/9950b7ad-7135-4d00-959f-1e450b3c1dcf)

**How It Works**  
GET `/images/search?breed_ids=INVALID999`. [perplexity](https://www.perplexity.ai/search/9950b7ad-7135-4d00-959f-1e450b3c1dcf)

**What's Checked**  
- Status 200.  
- Empty array or images without breed data.  
- Valid JSON format. [perplexity](https://www.perplexity.ai/search/9950b7ad-7135-4d00-959f-1e450b3c1dcf)

**Why It Matters**  
Prioritizes stability over strict validation. [perplexity](https://www.perplexity.ai/search/9950b7ad-7135-4d00-959f-1e450b3c1dcf)

## Test 6: Data Type Validation

**Purpose**  
Verifies correct formats: strings, numbers, secure URLs.

**How It Works**  
GET `/images/search?limit=1`. Checks first image properties.

**What's Checked**  
- ID: non-empty string.  
- URL: https://.  
- Width/height: positive numbers.  
- JSON Content-Type.

**Why It Matters**  
Catches bugs that break client apps.

## Test 7: Collection Variables

**Purpose**  
Uses shared collection settings for configurability.

**How It Works**  
GET `/images/search?breed_ids={{test_breed_id}}&limit={{default_limit}}`. Logs values.

**What's Checked**  
- Status 200.  
- Images ≤ `default_limit`.  
- Breed matches `test_breed_id`.

**Why It Matters**  
Makes tests reusable across environments.

## Test 8: Schema Validation

**Purpose**  
Validates full response against JSON Schema blueprint. [perplexity](https://www.perplexity.ai/search/13c387da-09a5-4177-bdef-9498e14c7c84)

**How It Works**  
GET `/images/search?limit=1`. Checks array structure, fields, types. [perplexity](https://www.perplexity.ai/search/13c387da-09a5-4177-bdef-9498e14c7c84)

**What's Checked**  
- Status 200.  
- Matches schema (id/url strings, width/height numbers ≥1, https URLs).  
- Logs detailed errors. [perplexity](https://www.perplexity.ai/search/13c387da-09a5-4177-bdef-9498e14c7c84)

**Why It Matters**  
Ensures consistent structure; catches API changes fast. [perplexity](https://www.perplexity.ai/search/13c387da-09a5-4177-bdef-9498e14c7c84)

**Notes**  
tv4 deprecation warning is cosmetic. [perplexity](https://www.perplexity.ai/search/13c387da-09a5-4177-bdef-9498e14c7c84)

## Test 9: Pre-request Script

**Purpose**  
Generates dynamic data (random limit, timestamp, header) before request. [perplexity](https://www.perplexity.ai/search/ddb56306-bb0e-4004-b56c-22fa19653c24)

**How It Works**  
Pre-request: Sets `random_limit` (1-10), `test_timestamp`, `X-Test-Run-ID`.  
GET `/images/search?limit={{random_limit}}`. API returns 10 regardless. [perplexity](https://www.perplexity.ai/search/ddb56306-bb0e-4004-b56c-22fa19653c24)

**What's Checked**  
- Status 200.  
- Exactly 10 images.  
- Header sent. [perplexity](https://www.perplexity.ai/search/ddb56306-bb0e-4004-b56c-22fa19653c24)

**Why It Matters**  
Automates setup for realistic, variable tests. [perplexity](https://www.perplexity.ai/search/ddb56306-bb0e-4004-b56c-22fa19653c24)

**Notes**  
API ignores limit without key; always 10 images. [perplexity](https://www.perplexity.ai/search/ddb56306-bb0e-4004-b56c-22fa19653c24)

## Test 10: Comprehensive Suite

**Purpose**  
Full health check: HTTP, structure, data quality. [perplexity](https://www.perplexity.ai/search/9950b7ad-7135-4d00-959f-1e450b3c1dcf)

**How It Works**  
Pre-request: Starts timer.  
GET `/images/search?limit=5`.  
Post: ~10 checks in groups. [perplexity](https://www.perplexity.ai/search/9950b7ad-7135-4d00-959f-1e450b3c1dcf)

**What's Checked**  
- HTTP: 200, <3000ms, JSON type.  
- Structure: Array, non-empty, ≤10.  
- Data: IDs unique, https .com URLs, all fields. [perplexity](https://www.perplexity.ai/search/9950b7ad-7135-4d00-959f-1e450b3c1dcf)

**Why It Matters**  
Quick regression for CI/CD; pinpoints issues. [perplexity](https://www.perplexity.ai/search/9950b7ad-7135-4d00-959f-1e450b3c1dcf)


