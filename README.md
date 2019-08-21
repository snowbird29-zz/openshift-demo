## OpenShift Origin Demo Setup

The demo setup consist of the following components, running inside the `ads-acc` CloudVPS tenant :


| Node        				| IP           	| Role 			| Release       | Remarks
| :-------------------------| :-------------| :-------------| :-------------| :-------------------|
| origin-master.acc.ads 	| 83.96.243.185 | API + etcd	| v3.11         | runs docker-registry
| origin-worker-1.acc.ads 	| 83.96.243.169 | Demo pods 	| v3.11         | runs HAProxy
| origin-worker-2.acc.ads 	| 83.96.200.22 	| Demo pods 	| v3.11         |
| origin-worker-3.acc.ads 	| 83.96.243.206	| Demo pods 	| v3.11         |
| origin-client.acc.ads		| 83.96.243.188 | Origin Client	| v3.11         | command line tools
| origin-nfs.acc.ads 		| 83.96.243.127 | Shared storage| n/a           | NFS

`Note`: demo setup is using a public IP address scheme, protected by CloudVPS security-group `openshift` and currently only accessible from within the Gemeente Amsterdam `194.13.133.0/29` network.

### Using the OpenShift Client

In order to create a new application, login to `origin-client.acc.ads` using your own CloudVPS SSH-key :

```bash
ssh <user_name>@origin-client.acc.ads.83.96.243.188.nip.io
```
From here you need to login into OpenShift first, using the following command :

```bash
oc login origin-master.acc.ads:443 -u <ssh_user_name> -p <ssh_user_name>
```

Once logged in succesful, you should get the following message :

```bash
Login successful.

You dont have any projects. You can try to create a new project, by running

    oc new-project <projectname>
```

`Note`: create a new project with project naming-convention `demo-<ssh_user_name>`

### Creating an Application From Source Code 

The `new-app` command allows you to create applications using source code in a local or remote Git repository.

To create an application using a Git repository, perform the following command :

```bash
oc new-app --strategy=docker|source <Git_Repo> 
```

`Note`: use `--strategy=docker` if you have a valid Dockerfile, otherwise use `--strategy=source`

In order to expose your shiny new App to the world, create a new route :

```bash
oc get svc
oc expose svc <your_new_svc>
oc get route
```

If everything went well and you do `NOT` run your container using privileged ports, you should be able to lookup your new App using cURL or a browser session using the output from the `oc get route` command.

[>>live example<<](http://signals-maintenance-page-demo-ga.83.96.243.169.nip.io/)

```bash
curl http://signals-maintenance-page-demo-ga.83.96.243.169.nip.io/
```

Or...check the status of your project, using the following commands :

```bash
oc status
oc get pods -o wide
oc logs <pod>
oc describe pod <pod>
```

### OpenShift Web Console

You can access the OpenShift Web Console [here](https://origin-master.acc.ads.83.96.243.185.nip.io)

`Note`: use your <ssh_user_name> as login credentials

### Support

For anything else behind this point, you most likely need additional support and/or coaching on the job. Feel free to contact me via Slack or [email](mailto:martijn.bakker@amsterdam.nl)

Or if you like, read the official [docs](https://docs.okd.io/3.11/welcome/index.html)

Team basis :rocket:
