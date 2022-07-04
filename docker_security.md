#Using Docker Bench to Enhance Container Security
Using acloudguru as a Lab/Learning environment. <>
1. Clone the docker bench repo from GitHub: `$ git clone https://github.com/docker/docker-bench-security.git`
2. Change your present working directory to docker-bench-security and run the docker-bench-security script.:
`$ cd docker-bench-security`
3. Using superuser permissions execute the docker-bench-security.sh shell script and redirect standard output to a file called /tmp/bench1.out
`$ sudo sh docker-bench-security.sh > /tmp/bench1.out`
4. After running the report, you may look at the contents with the Linux `more` command:
`more /tmp/bench1.out`
![alt text](https://github.com/secjedi/CyberDefense/blob/main/Images/zerologon/bench1.png) <br />
![alt text](https://github.com/secjedi/CyberDefense/blob/main/Images/zerologon/bench1_2.png) <br />
5. Update the audit rules on the server to include auditing the Docker Daemon
To list the rules already setup on the host, you may enter:
`sudo auditctl -l`
6. Use the auditctl command to add a rule to audit the Docker files in /var/lib/docker:
`sudo auditctl -w /var/lib/docker -k "docker lib"`
7. This will setup auditing on the docker daemon. To check you may enter the -l command again.
`sudo auditctl -l`
8. Run the Docker Bench security utility again and compare the output with the first run.
Now run the docker bench utility again and direct output to /tmp/bench2.out:
`sudo sh docker-bench-security.sh > /tmp/bench2.out`
![alt text](https://github.com/secjedi/CyberDefense/blob/main/Images/zerologon/bench2.png) <br />
9. To view the new report contanets, you may use the <code>more</code> command again.
`more /tmp/bench2.out`
10. Now use the Linux `diff` command to compare the output from the first run in bench1.out to the second run in bench2.out:
`diff /tmp/bench1.out /tmp/bench2.out`
![alt text](https://github.com/secjedi/CyberDefense/blob/main/Images/zerologon/diff.png) <br />

