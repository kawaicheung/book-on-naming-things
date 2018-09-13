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
