# SSh


## Intro to SSh


Hello and welcome to the very first part of the course. We're going to be covering quite a lot of topics

that span from Front-End to backend.

But, by the end of this course, you should be comfortable using all these tools and technologies.

Although these topics may seem quite different from one another,

the course was designed in a way to have everything flow together.

Although you can skip sections, I recommend you watch them in order because I show you how to connect

the dots on all of these technologies, and what you can do to build the great professional software and

the software that can compete with some of the biggest tech firms.

So hold on tight because we're going for a ride. The very first topic we're going to cover, is the exciting

topic of SSH. SSH or secure shell is a protocol.

What does that mean? You may have heard of other protocols like HTTP, HTTPS, FTP. These are all ways to

connect to computers and have a shared agreement of how to communicate.

That is their protocol or a language that they can speak and SSH is a way for machines to communicate

with one another.

For example, HTTP or Hypertext Transfer Protocol, allows you to send files over the internet like a HTML,

CSS, and javascript files between browsers and servers. FTP or file transfer protocol allows you to send

files as well and is often used when you upload files something like hostgator or a generic hosting

platform from your computer. HTTPS is similar to HTTP but it is encrypted.

That means third parties can't read the files being transferred

if they intercept the messages. And there are many other protocols like IMAP that allow users to send

e-mails and tons of others, and just like these, SSH is another protocol that allows us to communicate

between two computers over the Internet.

It allows users to share files as well as control and modify remote computers over the internet.

Sounds pretty cool right.

It was created as a secure way of communication which again encrypts all data so bad actors can't monitor

you, and don't worry I'll go deeper into each one of these points later on in the videos.

Now you might be thinking, OK, so what's the difference exactly from HTTPS?

I mean, they're both a form of communication and they're both encrypted.

Well just like a web browser uses the HTTPS protocol to talk with servers, and display Web sites.

A shell needs a certain protocol to enable data exchange or communication between two devices and not

just a browser and a server.

And that's why SSH is called secure shell protocol.

It's a protocol to use over shell and if you remember a shell unlike a browser allows you to talk to

the operating system. Yes, you've used your command line before right?

With SSH, I can use my terminal right over here which represents my computer.

To talk to another computer. Let's say this was another computer somewhere in the world.

Well, I can now communicate using SSH from my computer.

To this computer over here through our terminal.

Now the significant advantage offer by SSH over its predecessors is the use of encryption to ensure

secure transfer of information between host and the client.

The host refers to the remote server you're trying to access, while the client is the computer you're

using to access the host.

And as you can see that bad guy up there that looks very suspicious,

he can't read our messages because of encryption.

It's secure.

Now in the next video I'm going to show you how to use SSH.

And finally, talk about why this is useful for a web developer.

Trust me it's going to be fun. See you in the next one. Buh-Bye.


## Command Syntax

`ssh <user>@<hostname|IP>`

## Transfer file from your system to remote server using `rsync` command

So say u have folder called `myAwesomeSite` which has your file like `index.html`, `app.css` etc. and you want to transfer all those files to you remote server.

for that you can use `rsync` command like below:

`rsync -av <file|directory path> <username>@<ip>:<directory path>`

Example: say I am inside `myAwesomeSite` folder
$ rsync -av . root@192.168.99.57:~/repository/myAwesomeSite`

and it will transfer the file from your system to remote system.

## How SSh works ?
SSh uses three techniques:
* Symmetrical  Encryption
* Asymmetrical  Encryption and,
* Hashing

Encryption is a way to hide the data in meaningless form, so that unintended actors does read the message.

### Symmetrical Encryption: 
symmetrical Encryption uses the same key to encrypt and decrypt the message. So only the person who has the that encryption key can read that message.

but the problem here is the key itself can be compromise during the transfer/exchange of the key. so this techinique is atleast not used in standalone fashion.

### ASymmetrical Encryption:
uses two seperate keys for encryption and decryption key. These two keys are called as public and private keys. we share the public keys to anyone but we never ever share the private key, we keep it secret.

So the message which are `encrypted by public key` **can only be decrypted by** `private key` and theoritically its nearly imposible to calculate/find the private key based on public key.

as pert of Asummetrical Encryption, `Difiie Hellman key exchange algorithm` is used to exchange the public keys between two people.

### Hashing
All right what have we learned so far.

S-sh uses both symmetric and asymmetric encryption since asymmetric encryption is more time consuming.

Most of the S-sh connections use symmetric encryption.

As we've discussed the idea behind is that asymmetric encryption is used only to share a secret key.

And then finally using that key that is symmetric encryption for further communication so it's fast

now once a secure symmetric communication has been established the server uses the client's public key

and generate a challenge and transmitted to the client for authentication if the client can successfully

decrypt the message.

Well it means that it holds the private key required for a connection then the same session finally

begins.

So we finally have this secure tunnel everything's happy and we can communicate in a secure way.

One problem though.

Someone can sit in the middle and pretend to be the other and tamper or modify the message.

If they somehow convince the client that they're the host and the host that they're the client they

can just have this key exchange between one another and the information came flow through that middleman.

Now to solve one of these problems we'll talk about something called hashing and hashing is another

form of cryptography used in secure shell connections.

Now in my other course the complete web developer we use something called decrypt to hash passwords.

So you may remember that now one way hash functions differ from the above two forms of corruption.

The symmetrical and in asymmetric in that they're never meant to decrypt anything.

They simply generate a unique value of a fixed length for each input.

That A gets and there one way because if I hash Hello it's going to run through a function that spits

out jibberish really really fast with the trick being that we have no idea how to get back to the whole

text.

So if going through a hash.

If somebody looks at this they it's pretty much impossible for anybody to go back and figure out what

this means.

So how is that useful Well as we mentioned before.

S-sh is able to transmit messages securely but if a third party is able to dupe the client and the host

well they can tamper with the messages.

Maybe they could change the messages for nefarious reasons.

Luckily for us using the third technique hashes we're able to verify the authentication of the message

was so the bad person.

Well the bad person can modify our messages.

This is done using something called HMX or hash based Message Authentication codes.

I know it's a mouthful.

These names are getting really really confusing but the gist of it is this using a hash function each

message that is transmitted must contain a something called Mac and this Mac is a hash generated from

the symmetric key.

The packet sequence number and the message contents that were sent.

So let's say this client is trying to send the password with the host and they've already established

a symmetric key for secure communication while using the packet sequence number the key and then whatever

text I'm sending and this case let's just say we're sending the password.

I combine this into as an input into a hash function and this function is going to spit out some piece

of string.

That means absolutely nothing.

And now this is sent to the host.

But this means nothing right.

So how can the host check that the message hasn't been tampered with.

Well what they can do now is because they have the same information they can use their own symmetric

key which is the same as the clients.

They can use the packet sequence number again which they both know and then because this message was

sent through S-sh they also have that message that was sent.

So now they run it through the same hash function again.

And once it calculates it it's going to see does what I just generated match what the client hash was.

And if it does match well that means that this message was not tampered with because at any point if

the password even one letter even a capitalization was changed this hash will be completely different.

And that is one characteristic of hash functions change.

Any single thing no matter how similar the inputs.

Maybe it's going to spit out a completely different number now learning everything we have up until

this point.

We should understand what's going on when we type in S-sh at IP address or do you see up until now I

may have lied to you a little bit because everything that we've talked about if I go to my terminal

right now and we only implemented this well I would S-sh into that Digital Ocean server.

Press enter

and I get a password requirement but that didn't happen did it when I showed you in previous videos

I just immediately logged on and I was able to do anything that I wanted and like I said I cheated a

little bit here and I'll show you why that was and why we need to finally enter a password and the next

video.

I know a little bit of a cliffhanger.

I'll see in the next video.

Bebai.

### SSH or Password

* Difie-Hellman Key exchange
* Arrive at Symmetric Key
* Make sure of no funny buisiness (Hashing and MAC)
* Authenticate the user

### SSH into a server
elcome back.

All right.

Up until this point we learned that Diffie Helman key exchange provides for us a way to share a shared

secret.

I said share a lot there.

Oh well now we still need to authenticate.

So right now I could technically enter the password that was given to me by Digital Ocean and they won't

show up over here.

But let's try that again.

Password.

Copy and paste.

And I'll keep the password the same.

I'll have to change your password because they just gave me a temporary one so let's just keep it nice

and simple.

Oh boy they're asking a lot.

All right.

Finally were able to log in with our new password.

But as I said before that is a bit of a hassle.

You don't want to do that every time.

So using S-sh we're going to use something called RSA which allows us to provide or prove the identity

of the person without a password.

Sounds like magic doesn't it.

Let's get that set up and show you how we can do that.

So I'm going to exit out of here.

Let's clear this for now and I'm going to go into a folder that should exist on your computer if not

you may have to create this folder.

And the dot in front of it means that it's a hidden file so you may have to allow hidden files to even

be able to see it.

But if I just open this you'll see that is just a folder or like anywhere else on my computer my root

directory.

And if I look at the files in it I have a few things that you may not have because well this might be

your first time generating a necessary key.

In my case I already have some keys so there's some folders in there.

Don't worry if you don't see them.

Now if you see an I.D. or a safe file.

If not you'll have to generate one.

So let's generate a key just like we talked about in previous videos.

We're going to generate a public and private key just for Digital Ocean.

So that way with this computer with this laptop I can communicate with the server out there in the world

the way we can do that.

It's quite simple actually we can use the S-sh keys command which you should have on your computer if

not you may have to install the CLID where you can get it a few parameters.

There's a few that are recommended online but for now just to keep things simple we can just do

this with whatever your email addresses solve.

In my case I'm just going to do a test at gmail dot com wants to press enter.

You'll get to ask where you want to save the file and you see over here that it's saving get in my data

S-sh folder.

But I also don't want to.

I already have an ID or a say and I want specifically for Digital Ocean.

So I'm going to go ahead and just copy this and name it Digital Ocean.

So I know what the key is for going to press center and you have an option over here to enter a passphrase

just for extra security if you want.

In my case I don't really want to say you can just press enter enter and there you go.

We've just generated a public and private RSA keep.

Very cool.

And now let's clear this.

If I look at my folder I have the I.D. RSA Digital Ocean and Id say Digital Ocean and pop.

Now if this is your first time what S-sh.

Remember this.

If you remember anything from the section that is does pub which is a public key you can share that

with anybody that you want.

But this idea RSA Digital Ocean which is a private key never ever ever shared with anybody.

This is something that only you should keep on your computer.

Never ever share it otherwise.

Well pretty much all your encryption will be will be useless right.

So we were just able to generate our keys.

That's kind of cool right.

We can now copy this key and share it with our Digital Ocean server.

Remember in the diagrams we want to share our public keys by running the command and we can find this

online if you want.

So don't don't worry too much about it about it if you want to read up on what this command does.

It pretty much just copies whatever's inside though Id say disclosure in God PABX remember only the

public key that we're sharing.

So I'm going to press enter here and I've copied this to my clipboard.

Now if I go into S-sh to my Digital Ocean server see that I'm connected over here by the way if you're

wondering why I was able to log in this is because it's the same session actually exit the session it

will last me for the password to get.

For now we can do the same thing on this server.

We can make a directory called Dot S-sh just like we have on our computer.

And I can't spell.

So make directory and looks like the S-sh file already exists.

So let's just do Alice and you'll see over here that I can't see it.

That is because it is a hidden file remember I'll have to do a dash a to show our hidden Fox which is

S-sh over here.

Very cool.

Let me clear this.

Now we go into our DOT S-sh file and I cannot type.

Let's try that again.

All right.

We're here in our S-sh And if we look over here we have already just by default from Digital Ocean known

hosts and authorized keys.

If you don't see this based on whatever server you have you can just create the file.
But for now authorized keys is what we want.

So we can use something like nano which allows us to add a tax inside of authorized keys now in here

all we have to do is copy and paste our key that we had copied previously.

So this is our public key from our desktop that we just pasted in here with Neno you can see over here

to X it you have to hit command X or control X and then we want to modify and save We'll press Y and

then press enter.

All right so now if I do.

LS I see that I have authorized keys and in it we have our public infight exit.

Now well let's try this.

I'm going to S-sh into my root folder or let's end the session to make sure that it doesn't ask us for

a password and I'm going to see if I can enter it directly through S-sh with our keys without having

to worry about passwords.

So let's close this.

All right so as I say each and you see here that we just got a permission denied and that's a little

bit tricky here.

You may have this only if you're like me and in your S-sh it's all in your S-sh folder you have multiple

ID essays.

So you want to make sure that it's using the right public key for this connection.

So in our case we can just simply do this command which is as I say add and then grab the public key

that we just used and sorry grab your private key which you've created.

Press Enter Oh and you don't want to have your.

You want this.

So use  in case you have multiple keys in your `.ssh` folder, you will need to add the other keys whose name file name is not `id_rsa` using `ssh-add` command.
```ssh-add <your private key filename>```
And thats it your key is now added and you can 

you can aslo use
```pbcopy < public_key file_path```
to copy your public key to your clipboard.

Now say if you **deleted the publickey from your digitalocean server but you have the public and private keys in local system**  will u be able to access your digital ocean droploet using ssh ?

the answer is **No, you can not, since you already deleted the public key from authorized_keys file, now digital ocean does not have your public key. Hence it won't authneticate u and u ssh will fail.**

## Resources: SSH Into A Server
Recommended ssh-keygen  command:
```
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

Windows: 
If you have Git for Windows (which you should), ssh-keygen command should be available: https://gitforwindows.org/
You can read more about this here
Another option is to use https://www.ssh.com/ssh/putty/windows/puttygen

pbcopy  command: https://superuser.com/questions/472598/pbcopy-for-windows/1171448#1171448



Extra Video:
If you want to learn a little bit more about how SSH works internally, watch this excellent video: https://youtu.be/ORcvSkgdA58




