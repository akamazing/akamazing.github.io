## Observation summary: Ember Simple Auth


1. **Spoofing the External Web Application External Entity**   
**Mitigation strategy:** Use of standard authentication mechanism.   
**Observation:**  Ember Simple Auth implements standard authenticators like  OAuth 2, Torii, etc. Hence, it mitigates this threat. 

2. **Cross Site Scripting**  
**Mitigation strategy:** Sanitize user inputs.   
**Observation:**  Ember Simple Auth does not sanitize and verify user inputs. Potential to pursue a pull request or start a discussion on this topic but, needs more study.    

3. **Potential Data Repudiation by 1.0 Ember simple auth**   
**Mitigation strategy:** Generate logs to record the source, time, and summary of received data and exception handling   
**Observation:** Ember simple auth provides no mechanism to generate or, store logs. Third party addons "ember-inspector" exists that record promises and response resolution.       

4. **Elevation by Changing the Execution Flow in 1.0 Ember simple auth**   
**Mitigation strategy:** Implement authentication and authorize every request.   
**Observation:** In Ember Simple Auth  data is accepted only from authorized services of web application, thus this attack is mitigated.   

5. **Spoofing of the External Web Application External Destination Entity**   
**Mitigation strategy:** Implement authenticators.    
**Observation:**  Ember Simple Auth uses standard authenticator like OAuth2, Torii.   

6. **External Entity External Web Application Potentially Denies Receiving Data**    
**Mitigation strategy:** Generate logs or audits to record the source, time or summary of sent data and exception handling.   
**Observation:**  Ember Simple Auth has no provision to generate logs and audits, to record the source, time or summary of sent data, thus this threat exist.   
 
7. **The Local storage Data Store Could Be Corrupted**.  
**Mitigation strategy:** Validation of data received from the Session storage.   
**Observation:** Ember Simple Auth does not validate any data received from session storage. A discussion could potentially be started on sanitizing data before making requests.    

8. **Data Store Denies Local storage Potentially Writing Data**   
**Mitigation strategy:** Generate logs or audits to record the source, time or summary of sent data.   
**Observation:** Ember Simple Auth has no provision to generate logs and audits, to record the source, time or summary of sent data, thus this threat exist. Alternatively, failed promise handling mechanism could be used to send the request again.   

9. **Spoofing of Source Data Store Local storage**  
**Mitigation strategy:** Standard authentication to identify valid Session store.   
**Observation:** Ember Simple Auth has no authentication mechanism to identify valid data store.   

10. **Potential Data Repudiation by 1.0 Ember simple auth**   
**Mitigation strategy:** Generate logs or audits to record the source, time or summary of sent data.   
**Observation:** Ember Simple Auth has no provision to generate logs and audits, to record the source, time or summary of sent data, thus this threat exist.   

11. **Spoofing of the External API External Destination Entity**   
**Mitigation strategy:** Use of standard authentication mechanism.   
**Observation:** Ember Simple Auth uses standard authenticator like OAuth2, Torii. Hence, this threat is mitigated.   

12. **External Entity External API Potentially Denies Receiving Data**    
**Mitigation strategy:** Generate logs or audits to record the source, time or summary of sent data and exception handling.   
**Observation:** Ember Simple Auth has no provision to generate logs and audits, to record the source, time or summary of sent data, thus this threat exist. It does provide methods to resolve failed promise so requests can be made again.    

13. **Spoofing the External API External Entity**   
**Mitigation strategy:** Use of standard authentication techniques.   
**Observation:** Ember Simple Auth has no authentication mechanism to identify valid external API.   
