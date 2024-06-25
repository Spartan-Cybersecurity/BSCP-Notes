# Table of contents

* [La Biblia del Hacking en Web](README.md)
  * [ADVERTENCIA](la-biblia-del-hacking-en-web/advertencia.md)
  * [Conoce a tu academia](la-biblia-del-hacking-en-web/conoce-a-tu-academia.md)
  * [Conoce a tu instructor](la-biblia-del-hacking-en-web/conoce-a-tu-instructor.md)
  * [Aprende Hacking Web con los laboratorios de PortSwigger](la-biblia-del-hacking-en-web/aprende-hacking-web-con-los-laboratorios-de-portswigger.md)

## SQL Injection

* [¿SQL Injection?](sql-injection/sql-injection.md)
* [Lab 1: SQL injection vulnerability in WHERE clause allowing retrieval of hidden data](sql-injection/lab-1-sql-injection-vulnerability-in-where-clause-allowing-retrieval-of-hidden-data.md)
* [Lab 2: SQL injection vulnerability allowing login bypass](sql-injection/lab-2-sql-injection-vulnerability-allowing-login-bypass.md)
* [Lab 3: SQL injection attack, querying the database type and version on Oracle](sql-injection/lab-3-sql-injection-attack-querying-the-database-type-and-version-on-oracle.md)
* [Lab 4: SQL injection attack, querying the database type and version on MySQL and Microsoft](sql-injection/lab-4-sql-injection-attack-querying-the-database-type-and-version-on-mysql-and-microsoft.md)
* [Lab 5: SQL injection attack, listing the database contents on non-Oracle databases](sql-injection/lab-5-sql-injection-attack-listing-the-database-contents-on-non-oracle-databases.md)
* [Lab 6: SQL injection attack, listing the database contents on Oracle](sql-injection/lab-6-sql-injection-attack-listing-the-database-contents-on-oracle.md)

## Cross Site Scripting

* [¿XSS?](cross-site-scripting/xss.md)
* [Lab 1: Reflected XSS into HTML context with nothing encoded](cross-site-scripting/lab-1-reflected-xss-into-html-context-with-nothing-encoded.md)
* [Lab 2: Stored XSS into HTML context with nothing encoded](cross-site-scripting/lab-2-stored-xss-into-html-context-with-nothing-encoded.md)
* [Lab 3: DOM XSS in document.write sink using source location.search](cross-site-scripting/lab-3-dom-xss-in-document.write-sink-using-source-location.search.md)
* [Lab 4: DOM XSS in innerHTML sink using source location.search](cross-site-scripting/lab-4-dom-xss-in-innerhtml-sink-using-source-location.search.md)
* [Lab 5: DOM XSS in jQuery anchor href attribute sink using location.search source](cross-site-scripting/lab-5-dom-xss-in-jquery-anchor-href-attribute-sink-using-location.search-source.md)

## ClickJacking

* [¿Clickjacking?](clickjacking/clickjacking.md)
* [Lab 1: Basic clickjacking with CSRF token protection](clickjacking/lab-1-basic-clickjacking-with-csrf-token-protection.md)

## Access control vulnerabilities

* [¿Control de Acceso?](access-control-vulnerabilities/control-de-acceso.md)
* [Lab 1: Unprotected admin functionality](access-control-vulnerabilities/lab-1-unprotected-admin-functionality.md)

## Path traversal

* [¿Path Traversal?](path-traversal/path-traversal.md)
* [Lab 1: File path traversal, simple case](path-traversal/lab-1-file-path-traversal-simple-case.md)

## XML external entity (XXE) injection

* [¿XML external entity?](xml-external-entity-xxe-injection/xml-external-entity.md)
* [Lab 1: Exploiting XXE using external entities to retrieve files](xml-external-entity-xxe-injection/lab-1-exploiting-xxe-using-external-entities-to-retrieve-files.md)

## JWT

* [¿JWT?](jwt/jwt.md)
* [Lab 1: JWT authentication bypass via unverified signature](jwt/lab-1-jwt-authentication-bypass-via-unverified-signature.md)
* [Lab 2: JWT authentication bypass via flawed signature verification](jwt/lab-2-jwt-authentication-bypass-via-flawed-signature-verification.md)
* [Lab 3: JWT authentication bypass via weak signing key](jwt/lab-3-jwt-authentication-bypass-via-weak-signing-key.md)
* [Lab 4: JWT authentication bypass via jwk header injection](jwt/lab-4-jwt-authentication-bypass-via-jwk-header-injection.md)
* [Lab 5: JWT authentication bypass via jku header injection](jwt/lab-5-jwt-authentication-bypass-via-jku-header-injection.md)

## Server-side request forgery (SSRF)

* [¿SSRF?](server-side-request-forgery-ssrf/ssrf.md)
* [Lab 1: Basic SSRF against the local server](server-side-request-forgery-ssrf/lab-1-basic-ssrf-against-the-local-server.md)

## OS command injection

* [¿OS Command Injection?](os-command-injection/os-command-injection.md)
* [Lab 1: OS command injection, simple case](os-command-injection/lab-1-os-command-injection-simple-case.md)

## Authentication

* [¿Authentication?](authentication/authentication.md)
* [Lab 1: Username enumeration via different responses](authentication/lab-1-username-enumeration-via-different-responses.md)

## HTTP request smuggling

* [¿HTTP request smuggling?](http-request-smuggling/http-request-smuggling.md)
* [Lab 1: HTTP request smuggling, confirming a CL.TE vulnerability via differential responses](http-request-smuggling/lab-1-http-request-smuggling-confirming-a-cl.te-vulnerability-via-differential-responses.md)

## Server-side template injection

* [¿Server-side template injection?](server-side-template-injection/server-side-template-injection.md)
* [Lab 1: Basic server-side template injection](server-side-template-injection/lab-1-basic-server-side-template-injection.md)

## DOM-based vulnerabilities

* [Lab 1: DOM XSS using web messages](dom-based-vulnerabilities/lab-1-dom-xss-using-web-messages.md)
* [Lab 2: DOM XSS using web messages and a JavaScript URL](dom-based-vulnerabilities/lab-2-dom-xss-using-web-messages-and-a-javascript-url.md)

## WebSockets

* [Lab #1: Manipulating WebSocket messages to exploit vulnerabilities](websockets/lab-1-manipulating-websocket-messages-to-exploit-vulnerabilities.md)

## Prototype pollution

* [Lab 1: Client-side prototype pollution via browser APIs](prototype-pollution/lab-1-client-side-prototype-pollution-via-browser-apis/README.md)
  * [Utilizando DOM Invader](prototype-pollution/lab-1-client-side-prototype-pollution-via-browser-apis/utilizando-dom-invader.md)
* [Lab 2: DOM XSS via client-side prototype pollution](prototype-pollution/lab-2-dom-xss-via-client-side-prototype-pollution.md)
* [Lab 3: DOM XSS via an alternative prototype pollution vector](prototype-pollution/lab-3-dom-xss-via-an-alternative-prototype-pollution-vector/README.md)
  * [Utilizando DOM Invader](prototype-pollution/lab-3-dom-xss-via-an-alternative-prototype-pollution-vector/utilizando-dom-invader.md)
* [Lab 4: Client-side prototype pollution via flawed sanitization](prototype-pollution/lab-4-client-side-prototype-pollution-via-flawed-sanitization.md)
* [Lab 5: Client-side prototype pollution in third-party libraries](prototype-pollution/lab-5-client-side-prototype-pollution-in-third-party-libraries.md)
* [Lab 6: Privilege escalation via server-side prototype pollution](prototype-pollution/lab-6-privilege-escalation-via-server-side-prototype-pollution.md)

## GraphQL

* [Lab 1: Accessing private GraphQL posts](graphql/lab-1-accessing-private-graphql-posts.md)
