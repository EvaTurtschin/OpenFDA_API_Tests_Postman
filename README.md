# OpenFDA_API_Tests_Postman

Automated tests for the [OpenFDA API](https://open.fda.gov/apis/) using Postman and Newman.

---

## Project Structure

OpenFDA_API_Tests/
├─ collections/
│ └─ OpenFDA_Postman_Collection.json
├─ environments/
│ └─ OpenFDA_Postman_Environment.json
└─ README.md

## Requirements

- [Node.js](https://nodejs.org/) v18+  
- [Newman](https://www.npmjs.com/package/newman) (for running Postman collections from CLI)  

Install Newman globally if not installed:

```bash
npm install -g newman
```

## Runing Tests

### Run all tests from CLI

```bash
newman run collections/OpenFDA_Postman_Collection.json \
    -e environments/OpenFDA_Postman_Environment.json \
    -r cli,junit
```

```bash
-r cli,junit generates output in CLI and JUnit formats (optional for Jenkins reporting)
```

Make sure base_url and api_key are set in the environment file.

### Run specific folders (optional)

```bash
newman run collections/OpenFDA_Postman_Collection.json \
    -e environments/OpenFDA_Postman_Environment.json \
    --folder "Functional"
```

## Test Coverage

The collection includes tests grouped into folders:

- Setup – API key verification, basic ping requests.

- Smoke – Basic health checks for main endpoints.

- Functional – Search, pagination, count aggregation.

- Negative – Invalid parameters, invalid search syntax, error handling.

- Schema – Response structure validation, data type checks.

- Performance – Response time checks, load sanity tests.


## Notes

Tests are written in Postman scripting with proper handling of JSON parsing errors and dynamic variables.

### Environment variables include:

- base_url – OpenFDA API base URL

- api_key – Your OpenFDA API key

- limit_default – Default limit for requests

- first_page_ids – IDs from first page (used for pagination tests)

- smallResponseTime / largeResponseTime – Used in performance comparisons

## Jenkins Integration (Optional)

- Add repository to Jenkins.

- Install Node.js and Newman on the Jenkins node.

- Add a build step:

```bash
newman run collections/OpenFDA_Postman_Collection.json \
    -e environments/OpenFDA_Postman_Environment.json \
    -r cli,junit
```

- Use JUnit report for test results visualization.

### License

This project is for internal testing and learning purposes