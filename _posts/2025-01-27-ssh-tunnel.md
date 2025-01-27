To tunnel port 5601 (often used by Kibana) from a remote server to your localhost, you can use SSH port forwarding. Here’s how you can do it:

**Command Syntax**

```
ssh -L [LOCAL_PORT]:[REMOTE_HOST]:[REMOTE_PORT] [USER]@[REMOTE_SERVER]
```

**Specific Example for Kibana**

```
ssh -L 5601:127.0.0.1:5601 your-username@remote-server
```

**Explanation**

1. -L: Specifies the local port forwarding.
  
2. 5601:127.0.0.1:5601:
  

• 5601 (before :): The port on your localhost where you’ll access the service.

• 127.0.0.1:5601 (after :): The address and port of the service on the remote server.

3. your-username: Replace with your username for the remote server.
  
4. remote-server: Replace with the remote server’s IP or domain.
  

**Usage**

After running the command, you can open http://localhost:5601 in your browser to access Kibana (or the service running on port 5601).