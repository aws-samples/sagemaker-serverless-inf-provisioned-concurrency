## SageMaker Serverless Inference Provisioned Concurrency

Amazon SageMaker Serverless Inference is a purpose-built inference option that makes it easy for customers to deploy and scale ML models. Serverless Inference is ideal for workloads which have idle periods between traffic spurts and can tolerate cold starts. Serverless endpoints also automatically launch compute resources and scale them in and out depending on traffic, eliminating the need to choose instance types or manage scaling policies. 

Serverless Inference however can be prone to cold-starts, as if your serverless endpoint does not receive traffic for a while and then your endpoint suddenly receives new requests, it can take some time for your endpoint to spin up the compute resources to process the requests. In this notebook we specifically explore <b>Provisioned Concurrency</b>, a new feature in Serverless Inference which can help mitigate this issue. With Provisioned Concurrency you can keep the compute enviroment initialized and reduce cold-start as your serverless endpoint is kept ready. In this example we'll take a look at an example of deploying an On-Demand Serverless Endpoint vs a Provisioned Concurrency Enabled Serverless Endpoint.

## Provisioned Concurrency vs On-Demand Cold-Start Performance

To compare cold-start times we measure end to end latency by sending requests in 10 minute intervals. Here we send five requests with 10 minute intervals to ensure both the On-Demand and Provisioned Concurrency Endpoints are cold before each invoke.

```
# ~50 minutes
for i in range(5):
    time.sleep(600)
    start_pc = time.time()
    pc_response = runtime.invoke_endpoint(
        EndpointName=endpoint_name_pc,
        Body=b".345,0.224414,.131102,0.042329,.279923,-0.110329,-0.099358,0.0",
        ContentType="text/csv",
    )
    end_pc = time.time() - start_pc
    pc_times.append(end_pc)

    start_no_pc = time.time()
    response = runtime.invoke_endpoint(
        EndpointName=endpoint_name_on_demand,
        Body=b".345,0.224414,.131102,0.042329,.279923,-0.110329,-0.099358,0.0",
        ContentType="text/csv",
    )
    end_no_pc = time.time() - start_no_pc
    non_pc_times.append(end_no_pc)
```

## Security

See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.

## License

This library is licensed under the MIT-0 License. See the LICENSE file.

