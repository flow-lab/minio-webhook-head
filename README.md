### How to reproduce

1. Run docker compose `docker compose up`. 
2. Open console [http://localhost:9001](http://localhost:9001).
3. The minio-setup creates the webhook with auth_token poining to echo server.
4. The HEAD requests from MinIO server does not include authorization header with token (POST request does).

Expected: HEAD reqeust from MinIO to include the authorization header


LOG:
```shell
# HEAD request:
{"name":"echo-server","hostname":"4ef9e147e37d","pid":1,"level":30,"host":{"hostname":"backend","ip":"...","ips":[]},"http":{"method":"HEAD","baseUrl":"","originalUrl":"/events","protocol":"http"},"request":{"params":{},"query":{},"cookies":{},"body":{},"headers":{"host":"backend","user-agent":"Go-http-client/1.1"}},"environment":{"HOME":"/root","YARN_VERSION":"1.22.5","NODE_VERSION":"14.17.1","HOSTNAME":"4ef9e147e37d","PATH":"/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"},"msg":"Wed, 09 Mar 2022 12:57:20 GMT | [HEAD] - http://backend/events","time":"2022-03-09T12:57:20.696Z","v":0}

# POST request:
{"name":"echo-server","hostname":"4ef9e147e37d","pid":1,"level":30,"host":{"hostname":"backend","ip":"...","ips":[]},"http":{"method":"POST","baseUrl":"","originalUrl":"/events","protocol":"http"},"request":{"params":{},"query":{},"cookies":{},"body":{"EventName":"s3:ObjectCreated:Put","Key":"test-bucket/minio.png","Records":[{"eventVersion":"2.0","eventSource":"minio:s3","awsRegion":"","eventTime":"2022-03-09T12:57:20.836Z","eventName":"s3:ObjectCreated:Put","userIdentity":{"principalId":"minio"},"requestParameters":{"principalId":"minio","region":"","sourceIPAddress":"172.20.0.4"},"responseElements":{"content-length":"0","x-amz-request-id":"16DAB7BE17F64B32","x-minio-deployment-id":"c0fd1ea1-3397-4642-ac08-1b57f4cbcd35","x-minio-origin-endpoint":"http://172.20.0.3:9000"},"s3":{"s3SchemaVersion":"1.0","configurationId":"Config","bucket":{"name":"test-bucket","ownerIdentity":{"principalId":"minio"},"arn":"arn:aws:s3:::test-bucket"},"object":{"key":"minio.png","size":3539,"eTag":"85a37ef06ec1c24ae2dc88b33ded563c","contentType":"image/png","userMetadata":{"content-type":"image/png"},"sequencer":"16DAB7BE18433F11"}},"source":{"host":"172.20.0.4","port":"","userAgent":"MinIO (linux; arm64) minio-go/v7.0.22 mc/RELEASE.2022-02-16T05-54-01Z"}}]},"headers":{"host":"backend","user-agent":"Go-http-client/1.1","content-length":"1007","authorization":"Bearer token","content-type":"application/json"}},"environment":{"HOME":"/root","YARN_VERSION":"1.22.5","NODE_VERSION":"14.17.1","HOSTNAME":"4ef9e147e37d","PATH":"/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"},"msg":"Wed, 09 Mar 2022 12:57:22 GMT | [POST] - http://backend/events","time":"2022-03-09T12:57:22.951Z","v":0}
```


Note:
- docker compose version v2.2.1
