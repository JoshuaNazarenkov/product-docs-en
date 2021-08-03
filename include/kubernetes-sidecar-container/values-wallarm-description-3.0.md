```
wallarm:
  image:
     repository: wallarm/node
     tag: 3.0.0-3
     pullPolicy: Always
  # Wallarm API endpoint: 
  # "api.wallarm.com" for the EU cloud
  # "us1.api.wallarm.com" for the US cloud
  wallarm_host_api: "api.wallarm.com"
  # Username of the user with the Deploy role
  deploy_username: "username"
  # Password of the user with the Deploy role
  deploy_password: "password"
  # Port on which the container accepts incoming requests,
  # the value must be identical to ports.containerPort
  # in definition of your main app container
  app_container_port: 80
  # Request filtration mode:
  # "off" to disable request processing
  # "monitoring" to process but not block requests
  # "safe_blocking" to block malicious requests originated from greylisted IPs
  # "block" to process all requests and block the malicious ones
  mode: "block"
  # Amount of memory in GB for request analytics data,
  # recommended value is 75% of the total server memory
  tarantool_memory_gb: 2
```