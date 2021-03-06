# Code analysis for Ember-Simple-Auth

## Code review strategy

Ember simple auth (ESA) is a lightweight library for implementing authentication and authorization for EmberJS applications. Because of its lightweight nature and limited code we selected the following two strategies to review the code.

1. Scenario-based review
We focused on following scenarios, based on our misuse case and threat modeling for this review strategy.
* ESA should handle session management in a secure manner generating and authorizing session tokens
* ESA should not allow unauthenticated users to access data
* ESA should satisfactorily mitigate denial-of-services
* ESA should handle user data flow between Ember applications and server without interruption	

2. Weakness analysis review
ESA has several external dependencies. It depends on other authentication libraries like OAuth2, Torii, JavaScript libraries (like NodeJS, CORS, GLOB, Morgan, Express),  JQuery libraries and EmberJS. The weaknesses in these can become weaknesses for ESA as well. Thus, weakness analysis review of dependencies of ESA is crucial.   

## Manual code review of critical security functions identified in misuse cases and threat models

* __Total number of files reviewed__: 31
* __Total number of lines reviewed__: 3102

Following are the weaknesses found through manual code review:

1. Review authenticator for a potential denial of service attack.(CWE 285, 400)
Code in file ember-simple-auth/addon/authenticators/oauth2-implicit-grant.js is used for accessing client-side token as an argument ‘data’ when used to authenticate using Facebook, Gmail, Microsoft or GitHub account. 

'''
restore(data) {
    return new RSVP.Promise((resolve, reject) => {
      if (!this._validateData(data)) {
        return reject('Could not restore session - "access_token" missing.');
      }
      return resolve(data);
    });
  },
_validateData(data) {
	return !isEmpty(data) && !isEmpty(data.access_token);
}
'''

These functions are used to restore the session from a session data object. They will return a resolving promise when there is non-empty ‘access_token’ in the session data and a rejecting promise otherwise. The function does not verify the origin of data or the validity of request. The Eavesdropping by simple man-in-the-middle proxies can do a denial of service attack by sending random data to restore(data) and keeping the resources busy. 

2. ESA should satisfactorily mitigate denial of service (CWE 285, 400)
The code snippet in ember-simple-auth/addon/authorizers/oauth2-bearer.js is given below.
'''
	authorize(data, block) {
    const accessToken = data['access_token'];
    if (!isEmpty(accessToken)) {
      block('Authorization', `Bearer ${accessToken}`);
    }
  }
'''

This code is syntactically correct. But it deals with token-data that it get in ‘data’ parameters. We find that the only condition it checks is whether ‘accessToken’ variable is empty or not. If the source of ‘accessToken’ is not verified, there can be illegal and multiple attempts to insert tokens by third parties leading denial-of-service attack. 
	
## Automated code scanning

* To analyze the dependencies we used [snyk](https://snyk.io), that helped us to find vulnerabilities in dependencies pulled from npm. The full report of snyk analysis can be found [here](https://snyk.io/test/npm/ember-simple-auth?severity=high&severity=medium&severity=low).
* We also used [Retire.js](https://retirejs.github.io/retire.js/) to isolate the components with known vulnerability. The output from retire.js can be found [here](https://akamazing.github.io/reports/retireReport). 
* At last we used [JSLint](http://www.jslint.com) to scan the codes for any syntax error or bad practices.

Summary of key finding from manual and/or automated scanning. 
The weaknesses and vulnerabilities found in ESA library is low. We performed manual code review and automated scanning to find possible weaknesses. We were interested in finding the weaknesses related to improper handling of session tokens, authentication, authorization, and syntactical errors. ESA is quite small library with lots of dependencies. Most of these weaknesses are due to the weaknesses in other dependent libraries. We have summarized severity vulnerabilities identified by automated scanners below:
Vulnerable module: minimatch
Description: Regular Expression Denial of Service (DoS)
Severity: High
CWE Mappings: 400, 185.

Vulnerable module: growl 1.9.2
Description: Growl does not properly sanitize input before passing it to exec, allowing for arbitrary command execution.
Severity: High
CWE Mappings: 94, 77.

Vulnerable module: uglify-js 1.3.5
Description: Incorrect Handling of Non-Boolean Comparisons During Minification.
Severity: High
CWE Mappings: 241.

Manual code review found the following weaknesses :
Vulnerable module: ember-simple-auth/addon/authenticators/oauth2-implicit-grant.js, ember-simple-auth/addon/authorizers/oauth2-bearer.js
Description: Denial of Service (DoS)
Severity: High
CWE Mappings: 285, 400.

According to our observation, we conclude ESA to be an effective library for all authentication and authorization needs for ember app. It provides sufficient methods to use the implemented authorizers and authenticators like OAuth2, Torii as well as options to extend the existing classes to create custom authorizers and authenticators. ESA’s take on functionality is straightforward, that is to provide convenient methods for authentications and authorization and session management. Implementing security features we were looking for (i.e. input sanitization, parameter verification, channel encryption) either lies in the domain of developer of ember application or previously mentioned external framework. 


Links to any pull requests, issues, discussion, etc. from the team to the original project and any follow-up interactions. 

	Our plan was to send pull request for:

Vulnerable module: minimatch
Description: Regular Expression Denial of Service (DoS)
Severity: High
CWE Mappings: 400, 185, and 
Vulnerable module: ember-simple-auth/addon/authenticators/oauth2-implicit-grant.js, ember-simple-auth/addon/authorizers/oauth2-bearer.js
Description: Denial of Service (DoS)
Severity: High
CWE Mappings: 285, 400.

The first one was already opened and closed issue stating that the problem is due to minimatch rather than ESA library. 

The second is stated to be out-of-scope of ESA according to the discussion here. 

