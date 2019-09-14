<!-- sectionTitle: Performance Engineering -->

### ⏱
## Performance Engineering
Quickfire Edition

--- 
## What about Microservices/Serverless?
- Watch for timeouts
- Perform spike tests to determine time taken to scale

--- 

## What about GraphQL?
- It's possible, but more complicated
- Different combination of fields and fragments
- Make sure you have sensible request tracing

--- 

## Many birds, one stone 

### Reusable Tests. Framework selection is critical!

<br />

E2E test in Dev/Test Deployment
```bash
./gradlew gatlingRun -DbaseUrl="http://dev.env:80/api" -DnumberOfUser=1 -DrunDurationSecs=300
```

Perf Test
```bash
./gradlew gatlingRun -DbaseUrl="http://perf.env:80/api" -DnumberOfUser=2000 -DrunDurationSecs=3600
```

Smoke Test in Production
```bash
./gradlew gatlingRun -DbaseUrl="http://prod.env:80/api" -DnumberOfUser=1 -DrunDurationSecs=10
```


---
<!-- note
Cloudflare had a global performance degradation due to a small change to regex in their WAF rule set which caused CPU to become exhausted. 
-->
## CI/CD

- Build a small subset of your performance testing suite as part of your pipeline 
- Monitor the build time and capture performance metrics
- Fail or Add alert for any executions that over n %
- Run perf early in the SDLC and as often as possible

<br />

<small>

Cloudflare Outage July 2019 - https://blog.cloudflare.com/details-of-the-cloudflare-outage-on-july-2-2019/

</small>

---

## Test in Production?

### Yes, only if you can cleanup the data

Otherwise, test in a separate environment with configuration similar to prod
