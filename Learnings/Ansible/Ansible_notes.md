1. Ansible is open source tool, that helps automate configuration management, orchestration and deployment
2. Ansible is configuration management tool that works well with IAAC for updating,managing and patching, whereas terraform is only IAAC, used to provision cloud resources. 
3. Ansible can creates multiple environments and work well with multi-cloud environment as well
4. key advantage is that it operates in an agentless manner, establishing connections between nodes using SSH.
5. Ansible is a PUSH based mechanism, It pushes the updated to all server from parent, whereas CHEF is a pull based config management tool, that pulls the config from all children and then updates it
6. Ansible connects nodes, PUSHES small programs called modules to the nodes, and then removes them when they are done.
7. Need to install only on master node/instance/vm  and connects to rest of the server using SSH. To do that we need to create all aws instances using the same key-pair for SHH. Can be made using diff key-pair as well, but it becomes more complex to access


------------------------------------------
# Notes Links

https://intellipaat.com/blog/tutorial/devops-tutorial/ansible-basic-cheat-sheet/
https://medium.com/edureka/ansible-cheat-sheet-guide-5fe615ad65c0
