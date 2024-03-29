---
layout: post
title: "Maintain git repository in Linux"
description: This explains how a little kernel development know how will help in maintaining local clones of git repositories better, especially when working on many projects. 
subject: Maintaining local clones of git repositories in Linux
apple-title: Maintain git repository in Linux
app-name: Properly maintaining local clones of git repositories in Linux 
tweet-title: Maintain git repositories in Linux
tweet-description: This explains how little kernel development know how will help in managing local clones of git repositories better, especially when working on many projects.
og-title: Maintian git repositories in Linux
og-url: blog/2016/06/11/maintain-git-repository-in-linux/
canonical-url: blog/2016/06/11/maintain-git-repositories-in-linux
date: 2016-06-11
keywords: git, repository, manage , maintain, maintaining git repository, manage git repositories, manage git repository in linux,maintain git repository in linux server,manage git repo
---

Maintaining local clones of git repositories in Linux could become tiresome especially if you are a software developer who:

* usually works in slow Internet Connection and has low Internet bandwidth
* switches a lot between various systems having windows OS or Linux or both
* is working on a wide range projects all in different domains.
* is using <a href="http://puppylinux.com">Puppy Linux</a> ( mostly ) for Software development.
* has standard  (frugal) installation of <a href="http://puppylinux.com" >Puppy Linux</a> or frugal installation of any other Linux Distro.
* needs to move a lot of git repositories around the system. 

Now, <a href="https://git-scm.com">git</a> is a wonderful tool to maintain your software's source code. Agreed. I prefer it over [svn](https://en.wikipedia.org/wiki/Apache_Subversion) and [mercurial](https://en.wikipedia.org/wiki/Mercurial) too. But maintaining a lot of local clones can sometimes become a nightmare.

Say for instance, you have 50 git repositories, and you have to move all of them to different location . What do you do ?? Write a quick bash script to do the job ?? Well you could, but then you will have reconfigure those individual repositories.

 Tiresome isn't it ?

Well, it was for me too, Luckily I was helped by one of my friend who actually introduced me to Linux long back. She had suggested me to use separate partition file for all my git repositories, which is what I am going to explain in this blog post.

<section>
<h2>Creating space for git repositories</h2>
<p>First of all we will be needing a space where we could keep all your git repositories. This space would be special as moving it will not change the git history of the repositories residing in it.</p>

<section>
<h3> Creating a Raw Image </h3>
<p> Start off by making a raw image: </p>

<pre>
<code>
#root dd if=/dev/zero of=git_repo.4fs bs=1M count=32
</code>
</pre>
<p>We will get some output such as this:</p>
<pre>
<code>
32+0 records in
32+0 records out
33554432 bytes (34 MB) copied, 0.0963162 s, 348 MB/s
</code>
</pre>
<p>This creates (34 MB) of raw image.<br>
After this if we check our current folder in which we ran this command we will get a file named <code>git_repo.4fs</code>.</p>

<pre>
<code>
root# pwd 
/initrd/mnt/dev_save
</code>
</pre>

<p>Checking with <code>ls</code> or using GUI to navigate to that folder and we will find the file. </p>

<pre>
<code>
root# ls -las
</code>
</pre>

<p>With this we find the file name in the output somewhere.</p>

<pre>
<code>
    4 drwxr-xr-x  3 root root       4096 Jun 10 16:25 .
    4 drwxr-xr-x 14 root root       4096 Jun  9 16:50 ..
32768 -rwxr-xr-x  1 root root   33554432 Jun 10 16:25 git_repo.4fs
</code>
</pre>

</section>
<section>
<h3> Making a Partition File </h3>
<p>Now we have a raw image which we could hopefully convert to a partition file... </p>
<p>To convert it to a partition file we will make use of <code>mkfs</code> command. As we have put in the extension of file as <code>.4fs</code> we will be converting this to an <a href="https://en.wikipedia.org/wiki/Ext4">ext4</a> (file) partition.</p>

<pre>
<code>
root# mkfs -t ext4 git_repo.4fs
</code>
</pre>

<p>We will be asked that this not a block special device, and that would we want to proceed anyways ? Confirm by putting in <code>y</code> and pressing enter.</p>

<pre>
<code>
mke2fs 1.42.9 (4-Feb-2014)
git_repo.4fs is not a block special device.
Proceed anyway? (y,n) y
</code>
</pre>

<p>If successful, we will get an output as such:</p>

<pre>
<code>
Filesystem label=
OS type: Linux
Block size=1024 (log=0)
Fragment size=1024 (log=0)
Stride=0 blocks, Stripe width=0 blocks
8192 inodes, 32768 blocks
1638 blocks (5.00%) reserved for the super user
First data block=1
Maximum filesystem blocks=33554432
4 block groups
8192 blocks per group, 8192 fragments per group
2048 inodes per group
Superblock backups stored on blocks: 
       8193, 24577

Allocating group tables: done
Writing inode tables: done
Creating journal (4096 blocks): done 
Writing superblocks and filesystem accounting information: done
</code>
</pre>

<p>Now our file partition is ready. If you are using <a href="http://puppylinux.com" >Puppy Linux</a> , just click the file from GUI and it will be mounted like any partition would. On the other hand, if you are using any other distro of Linux, you would have to mount it using <code>mount</code> command.</p>

</section>
<section>
<h3> Mounting and unmounting the partition file </h3>

<p>To mount the partition file, first start of by making a empty directory in <code>/mnt/</code> folder.</p>

<p>Change to <code>mnt</code> directory if you are not</p>

<pre>
<code>
cd /mnt
</code>
</pre>

<p>Then, by confirming that we are the correct place make a directory called <code>git_repos</code></p>

<pre>
<code>
root# pwd
/mnt
root# mkdir git_repos
root# ls
cdrom  dvd    floppy     home                     msdos  swap
data   flash  git_repos  ram1   zip
</code>
</pre>

<p>Here we could mount our partition file. I have kept the partition file in <code>home</code> directory (where all the sfs, personal save file and all the other files of <a href="http://puppylinux.com">Puppy Linux</a> are kept), so my mount command would look something like this:</p>

<pre>
<code>
root# mount home/git_repo.4fs git_repos
</code>
</pre>

<p>After changing to <code>git_repos</code> directory we would see something like this:</p>

<pre>
<code>
root# cd git_repos
root# ls
lost+found
</code>
</pre>

<p>To unmount simply use <code>umount</code> :</p>

<pre>
<code>
cd /mnt
umount git_repos
</code>
</pre>

<p>And that's all , we have a partition file where you could keep your git repositories ! Move this partition file around without effecting the history of the git repositories.</p>


</section>
</section>
<section>
<h2>Working with your git repositories</h2>
<p> Now we have space to keep all the git repositories. However, as this is a partition file you have to treat it as any partition in your drive. Only the plus point is we could move it around anywhere like a directory.</p>

<section>
<h3> Resizing the file partition </h3>

<p>That said , what if we want to increase the size accomodate more git repositories later on ?? That could be done by using <code>resize2fs</code>. But before doing that run <code>e2fsck</code> over it as such:</p>

<pre>
<code>
root# e2fsck -f git_repo.4fs
e2fsck 1.42.9 (4-Feb-2014)
Pass 1: Checking inodes, blocks, and sizes
Pass 2: Checking directory structure
Pass 3: Checking directory connectivity
Pass 4: Checking reference counts
Pass 5: Checking group summary information
git_repo.4fs: 9749/51200 files (0.4% non-contiguous), 121125/204800 blocks

</code>
</pre>

<p>Then use <code>resize2fs</code>. Say if we want the partition file to have 512mb of space, so the command of <code>resize2fs</code> would look something like this:</p>

<pre>
<code>
root# resize2fs git_repo.4fs 512m
resize2fs 1.42.9 (4-Feb-2014)
Resizing the filesystem on howling.4fs to 524288 (1k) blocks.
The filesystem on git_repo.4fs is now 524288 blocks long.
</code>
</pre>

</section>
<section>
<h3> Maintaining the file partition </h3>

<p>If you have abrupt power cuts, as your system shuts down or crashes suddenly while you were working with your file partition mounted, remember to <code>e2fsck</code> first to repair the file system before mounting it again.</p>

<pre>
<code>
root# e2fsck git_repo.4fs
e2fsck 1.42.9 (4-Feb-2014)
git_repo.4fs: clean, 11/8192 files, 5530/32768 blocks
</code>
</pre>
</section>
</section>

If you find any mistakes in the article send me a [pull request here](https://github.com/Mr-Kumar-Abhishek/Mr-Kumar-Abhishek.github.io).
