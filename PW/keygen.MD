## Generate an access public key to login to cloud instances on Parallel Works

### Unix/MacOS
Tutorial at https://www.ssh.com/academy/ssh/keygen

From the computer that you will use to login to Parallel Works, run the command `ssh-keygen` on the terminal (for Unix/MacOS) or command prompt (for Windows) without any arguments. You will get a prompt for the file in which to store the generated keys: 

```
Enter file in which to save the key (/Users/user/.ssh/id_rsa):
```

Next, you will get a prompt for a passphrase. 
```
Enter passphrase (empty for no passphrase):
```
> The passphrase is used for encrypting the key, so that it cannot be used even if someone obtains the private key file. The passphrase should be cryptographically strong.

Two key files will be created - the private key file (e.g. `id_rsa`) and the public key file (e.g. `id_rsa.pub`). Open the public key file (e.g. `id_rsa.pub`) in a text editor and copy and paste the contents to the "Access Public Key" box on Parallel Works Edit Resource page.

The public key file will look like a longer version of this:

```
ssh-rsa AABAB3ZzQbevrCC9/K6g529KVeW+nxE5cbgSf30SqPRYGxkeLuT8VnBnJpiuk= user@computer
```

If you don't want to add the access key every time you create a new instance, you can go to your Account menu, then the Authentication tab and add your public key with the +Add SSH Key button. This will automatically add your key to every new instance, and you can have multiple keys added to your account.
