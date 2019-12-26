# jira-oauth

[![Build Status](https://api.travis-ci.org/nortonlifelock/jira-oauth.svg?branch=master)](https://travis-ci.org/nortonlifelock/jira-oauth)
[![Go Report Card](https://goreportcard.com/badge/github.com/nortonlifelock/jira-oauth)](https://goreportcard.com/report/github.com/nortonlifelock/jira-oauth)
[![GoDoc](https://godoc.org/github.com/nortonlifelock/jira-oauth?status.svg)](https://godoc.org/github.com/nortonlifelock/jira-oauth)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

Using the Go Generate Cert code: https://golang.org/src/crypto/tls/generate_cert.go

Build the code and execute the following command
```
./generate_cert --host="source:destination" -rsa-bits 1024
 
openssl pkcs8 -topk8 -nocrypt -in key.pem -out jira_privatekey.pcks8
  
openssl x509 -pubkey -noout -in cert.pem  > jira_publickey.pem
```
This will generate the necessary files for use with the JIRA OAuth and Aegis system

To pull the private key for Aegis execute
cat key.pem

The returned key needs to be encrypted using the kms_tool and stored in the SourceConfig table in the PrivateKey column for the JIRA connection

To Pull the public key for JIRA execute 
cat jira_publickey.pem

Following the instructions for setting up the Application Link in JIRA at this link: https://developer.atlassian.com/server/jira/platform/oauth/

The ConsumerKey should be a randomly generated 30 character password

The ConsumerName should be relevant to the application link being created

The PublicKey should be the out put from the cat of the jira_publickey.pem file

After saving this entry in JIRA and adding the private key and consumer key to the Database post encryption then it's time to run the jira_oauth tool.

In JIRA sign into the account that should be used for this application link. 

This tool must be run on a system with access to the Aegis database that's being setup. Running this the first time will generate a link to a JIRA page. Upon following this link click "Accept" then using the password generated in the following page enter that into the jira_oath tool and hit enter. This registers the token in the database for subsequent calls into the JIRA api.
