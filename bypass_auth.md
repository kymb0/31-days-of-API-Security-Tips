# How to bypass object level authorization:

 * Wrap the ID with an array.
 * Instead of {“id”:111} send {“id”:[111]}
 * Wrap the ID with a JSON object
 * Instead of {“id”:111} send {“id”:{“id”:111}}
 * Try to perform HTTP parameter pollution:  
 /api/get_profile?user_id=<legit_id>&user_id=<victim’s_id>  
 OR  
 /api/get_profile?user_id=<victim’s_id>&user_id=<user_id>  
 This can work in situations where the component that performs the authorization check and the endpoint itself use different libraries to parse query parameters. In some cases, library #1 would take the first occurence of “user_id” while library #2 would take the second.
 * Try to perform JSON parameter pollution
 POST api/get_profile  
 {“user_id”:<legit_id>,”user_id”:<victim’s_id>}  
 OR  
 POST api/get_profile  
 {“user_id”:<victim’s_id>,”user_id”:<legit_id>}  
 It’s pretty similar to HTTP parameter pollution.  
 * Try to send a wildcard instead of an ID. It’s rare, but sometimes it works.
 * Find API hosts where the authorization mechanism isn’t enabled. Sometimes, in different environments, the authorization mechanism might be disabled, for various reasons. For example: QA would be vulnerable but production would not.
 * Try to perform type-juggling  
 Not common, but a great example of how you can bypass BOLA protection
