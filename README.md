# OpenFDA_API_Tests_Postman

Automated tests for the [OpenFDA API](https://open.fda.gov/apis/) using Postman and Newman, with CI/CD integration via GitHub Actions and Allure reports.

---
## Project Structure

```bash
OpenFDA_API_Tests/
├─ collections/
│  └─ OpenFDA_Postman_Collection.json
├─ environments/
│  └─ OpenFDA_Postman_Environment.json
├─ workflows/
│  └─ run-tests.yml
└─ README.md
```

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
The -r cli,junit option generates output in CLI and JUnit formats (useful for CI reporting).

### Run specific folders (optional):

```bash
newman run collections/OpenFDA_Postman_Collection.json \
    -e environments/OpenFDA_Postman_Environment.json \
    --folder "Functional"
```

Ensure that base_url and api_key are set in the environment file before running.

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

## CI/CD with GitHub Actions

The project uses GitHub Actions for automated testing on each push or pull request.

### Workflow highlights:

- Run Newman tests – Executes all Postman tests.

- Generate Allure report – Collects test results and generates a visual report.

- Deploy to GitHub Pages – Allure report is published automatically.

### Viewing the Allure Report

After workflow execution, the report is accessible at:

[Open Allure Report](https://evaturtschin.github.io/OpenFDA_API_Tests_Postman/)

## Notes on CI Behavior

- The Response Time check is live; it may occasionally warn if responses are slow.

- Negative tests allow 200 OK responses if the API does not strictly enforce validation.

## Optional Jenkins Integration

- Add the repository to Jenkins.

- Install Node.js and Newman on the Jenkins node.

- Add a build step:

```bash
newman run collections/OpenFDA_Postman_Collection.json \
    -e environments/OpenFDA_Postman_Environment.json \
    -r cli,junit
```

- Use the JUnit report for test results visualization.

### License

This project is for internal testing and learning purposes
