Another example of naming, creating an object...


Rate limit object

What do i want this object to feel like? What's exposed wahts not?

originally have the datetime passed in, but does the implementer reqlly care? API implementer cares that x number of requests happened in last y min,
not about dates...

so hide the dates in the object....

Naming of constructor... at first

  public APIRateLimitInfo(DateTime expiration_date)
        {
            ExpiresOn = expiration_date;
        }
        
  public APIRateLimitInfo(int expiration_date_in_min_from_now)
        {
            ExpiresOn = DateTime.UtcNow.AddMinutes(expiration_date_in_min_from_now);
        }
  public APIRateLimitInfo(int minutes_until_expired)
        {
            ExpiresOn = DateTime.UtcNow.AddMinutes(minutes_until_expired);
        }
        
        
Checks for "Expires"...

Naming things to inform how we really care to use them...what bit of info the implementer really cares about and shouldnt care about even tho while building this thing, 
the 'date' is of importance since we're erecting this thing. when using, date should vanish...

minutes_until_expired vs. expires_on_in_min -- latter suggests the 'thing that's beign checked', not the 'question' we care about. subtle.


Show: IsOverAPIRateLimit() method so we can see the readability. Explain the context...moving the details out of the rate limit attribute into a service method...

But now IsOverAPIRateLimit() is a weird name since we are also incrementing the counter if we're ok. so really it should be something like "IsAPIRequestOverRateLimit()" tells we're considering this check as a request... is it single-repsonsbility? questionsable but its SO tightly coupled that i say its ok. it's ostensibly doing what it says -- checking this current api request is under the rate limit. the imp has a couple things going on -- a get of the rate limit object and a setting, but that's all in the details of hte imp. it's a 'single question' for the requester.

well shit...need to expose the 'retry-after'...so move this to "APIRequestMustWaitXSeconds"??
