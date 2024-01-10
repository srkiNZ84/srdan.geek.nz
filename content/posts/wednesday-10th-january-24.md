+++ 
draft = true
date = 2024-01-10T16:54:55+13:00
title = "Git SSH authentication and troubleshooting"
description = "Getting SSH authentication working with GitLab and GitHub and several profiles"
slug = ""
authors = ["Srđan Đukić"]
tags = ["git", "GitHub", "GitLab", "SSH"]
categories = []
externalLink = ""
series = []
+++
# Git SSH authentication

Needed to add:

  sshCommand = "ssh -o IdentitiesOnly=yes -i ~/.ssh/gitlab_srkiNZ_id_rsa -F /dev/null"

to .git/config as per: https://docs.gitlab.com/ee/user/ssh.html#use-different-keys-for-different-repositories in order
to get git to not use the SSH agent (or is it the credentials helper?).

Also used: 

ssh -T git@gitlab.com
Welcome to GitLab, @srkiNZ!

to debug and figure out which bloody SSH identity was being used.
