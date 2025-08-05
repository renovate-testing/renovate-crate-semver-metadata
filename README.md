# minimal-reproduction-template

First, read the [Renovate minimal reproduction instructions](https://github.com/renovatebot/renovate/blob/main/docs/development/minimal-reproductions.md).

Then replace the current `h1` with the Renovate Issue/Discussion number.

## Current behavior

Renovate removes SemVer metadata from crate versions when looking up crate information.

When attempting to update from [`phonenumber@0.3.6+8.13.36`](https://crates.io/crates/phonenumber/0.3.6+8.13.36) to [`phonenumber@0.3.7+8.13.52`](https://crates.io/crates/phonenumber/0.3.7+8.13.52), the following request fails:

```
DEBUG: GET https://crates.io/api/v1/crates/phonenumber/0.3.7 = (code=ERR_NON_2XX_3XX_RESPONSE, statusCode=404 retryCount=0, duration=18)
WARN: Release interceptor failed for "crate"
{
  "err": {
    "name": "HTTPError",
    "code": "ERR_NON_2XX_3XX_RESPONSE",
    "timings": {
      "start": 1754384095852,
      "socket": 1754384095853,
      "lookup": 1754384095853,
      "connect": 1754384095853,
      "secureConnect": 1754384095853,
      "upload": 1754384095853,
      "response": 1754384095869,
      "end": 1754384095870,
      "phases": {
        "wait": 1,
        "dns": 0,
        "tcp": 0,
        "tls": 0,
        "request": 0,
        "firstByte": 16,
        "download": 1,
        "total": 18
      }
    },
    "message": "Response code 404 (Not Found)",
    "stack": "HTTPError: Response code 404 (Not Found)\n    at Request.<anonymous> (/usr/local/renovate/node_modules/.pnpm/got@11.8.6/node_modules/got/dist/source/as-promise/index.js:118:42)\n    at processTicksAndRejections (node:internal/process/task_queues:105:5)",
    "options": {
      "headers": {
        "user-agent": "RenovateBot/41.51.1 (https://github.com/renovatebot/renovate)",
        "accept": "application/json",
        "accept-encoding": "gzip, deflate, br"
      },
      "url": "https://crates.io/api/v1/crates/phonenumber/0.3.7",
      "hostType": "crate",
      "username": "",
      "password": "",
      "method": "GET",
      "http2": false
    },
    "response": {
      "statusCode": 404,
      "statusMessage": "Not Found",
      "body": {
        "errors": [
          {
            "detail": "crate `phonenumber` does not have a version `0.3.7`"
          }
        ]
      },
      "headers": {
        "content-type": "application/json",
        "content-length": "81",
        "connection": "keep-alive",
        "access-control-allow-origin": "*",
        "content-encoding": "br",
        "date": "Tue, 05 Aug 2025 08:54:55 GMT",
        "nel": "{\"report_to\":\"heroku-nel\",\"response_headers\":[\"Via\"],\"max_age\":3600,\"success_fraction\":0.01,\"failure_fraction\":0.1}",
        "report-to": "{\"group\":\"heroku-nel\",\"endpoints\":[{\"url\":\"https://nel.heroku.com/reports?s=%2BrLB%2B4RaGuBn6%2FPlUKLUZx8DmrCmXVIRoWkmi%2BjVdSY%3D\\u0026sid=af571f24-03ee-46d1-9f90-ab9030c2c74c\\u0026ts=1754384095\"}],\"max_age\":3600}",
        "reporting-endpoints": "heroku-nel=\"https://nel.heroku.com/reports?s=%2BrLB%2B4RaGuBn6%2FPlUKLUZx8DmrCmXVIRoWkmi%2BjVdSY%3D&sid=af571f24-03ee-46d1-9f90-ab9030c2c74c&ts=1754384095\"",
        "server": "Heroku",
        "strict-transport-security": "max-age=31536000; includeSubDomains",
        "via": "1.1 heroku-router, 1.1 d125bf8405e840aa51a88ae3d8d91fb2.cloudfront.net (CloudFront)",
        "vary": "accept-encoding",
        "x-cache": "Error from cloudfront",
        "x-amz-cf-pop": "IAD12-P1",
        "x-amz-cf-id": "BMf7mHvVvvTgbRp8fTjUjdivrkCN7gmgOeLv4LGmqd7rpRlXWejxRg=="
      },
      "httpVersion": "1.1",
      "retryCount": 0
    }
  }
}
```

## Expected behavior

Renovate should include SemVer metadata when looking up crate information.

Current:

```
GET https://crates.io/api/v1/crates/phonenumber/0.3.7
```

Expected:

```
GET https://crates.io/api/v1/crates/phonenumber/0.3.7+8.13.52
```

Cargo ignores SemVer metadata ([docs](https://doc.rust-lang.org/cargo/reference/specifying-dependencies.html#version-metadata)), but the full SemVer version string is required for registries like [crates.io](https://crates.io).

## Link to the Renovate issue or Discussion

Put your link to the Renovate issue or Discussion here.
