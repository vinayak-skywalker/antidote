# Working with Salt
## Part 1 - Salt Master and Minion

[Salt](https://saltstack.com/) is one of the most powerful, scalable, and flexible platforms
that allows you to automate key operational and configuration tasks in your
network. Salt is open source and it comes packaged with modules supporting Junos
OS right out of the box.

Salt has a server-agent architecture where the Salt Master is the server and the agent is installed in the Salt Minions. The role of the Salt Master is to manage the state of the infrastructure. 

The Salt Master and the Salt minion can run on seprate machines or can run on the same machine itself. In our case, the Salt Master and Minion are running on the same machine.

Now let's start the Salt Master.

```
service salt-master restart
```
<button type="button" class="btn btn-primary btn-sm" onclick="runSnippetInTab('salt1', 0)">Run this snippet</button>

We have to configure the Salt Minion via /etc/salt/minion
```
master: salt1
id: minion
```
```
cat /etc/salt/minion
```
<button type="button" class="btn btn-primary btn-sm" onclick="runSnippetInTab('salt1', 2)">Verify Output (Optional)</button>

Next let's start the Salt Minion.

```
service salt-minion restart
```
<button type="button" class="btn btn-primary btn-sm" onclick="runSnippetInTab('salt1', 3)">Run this snippet</button>

Once the Salt minion is running, it will send its public key to the Salt Master. We can view the key's status by executing "salt-key -L". 

( Note: it might take sometime for the key to be populated. Keep executing the below command every few seconds, until you see the "minion" key listed. )
```
salt-key -L
```
<button type="button" class="btn btn-primary btn-sm" onclick="runSnippetInTab('salt1', 4)">Run this snippet</button>

Let's accept the Salt Minion's public key using the command

```
salt-key --accept="minion" -y
```
<button type="button" class="btn btn-primary btn-sm" onclick="runSnippetInTab('salt1', 5)">Run this snippet</button>

Once this is done, the Salt Master will be able to communicate with the Salt Minion and issue remote commands.

Next, we will run the test.ping command to ensure that the Salt Minion is connected and is responding to the Salt Master by using the 
(Note: give a few seconds for the keys to sync, before trying this command) 
```
salt '*' test.ping
```
<button type="button" class="btn btn-primary btn-sm" onclick="runSnippetInTab('salt1', 6)">Run this snippet</button>

We can use the cmd.run execution module to run a remote command on the Salt Minion. In this case, we're checking what version of python is running on the Salt Minion.

```
salt minion* cmd.run 'python -V'
```
<button type="button" class="btn btn-primary btn-sm" onclick="runSnippetInTab('salt1', 7)">Run this snippet</button>

