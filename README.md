This repository illustrate the process of configuring remote debugging for a go application running on a kubernetes cluster.
<h2> Instructions </h2>
  
The Dockerfile builds a production version of the application and the Dockerfile.debug builds a debug version that will be started under Delve with remote debugging enabled and listening on port 40000 for the debugger.

The sample.yaml deploys the production code and sample_debug.yaml deploys the debug version. Other than the container the debug version exposes a the debug port as a service. This can be removed and a port forward used if desired.

If you are using VS Code as a debugger you will need to add a remote debug launch config, the one I used is below, you will need to change the host.

```yaml
    {
        "name": "Connect to server",
        "type": "go",
        "request": "attach",
        "mode": "remote",
        "remotePath": "",  
        "cwd": "${workspaceFolder}",     
        "port": 40000,
        "host": "192.168.0.2",
        "showLog": true,
        "apiVersion": 2,
        "trace": "verbose"
    }
```

Note that the application will not start until a debugger conects and Delve can be quite sensitive. If you don't connect soon after the pod is deployed it may fail to connect. I have found that the connection will occaisionally fail as well so try a few times if things dont seem to work.

Good luck!
