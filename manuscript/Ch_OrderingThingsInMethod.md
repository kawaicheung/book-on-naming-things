Just about the way params are used, passed, constructed, hydrated...keep consistent if possible:

Everything here is name, then email, then timezone, where possible
```
public void UpdateProfile(
            int user_id,
            string name,
            string email_address,
            string timezone)
        {
            using (var context = X2O.Helpers.DB.GetContext(_connection_string))
            {
                name = name.Trim();
                email_address = email_address.Trim().ToLower();

                #region input validation

                if (!Validation.IsBetweenCertainLength(name, 1, 255))
                {
                    throw new Domain.Exceptions.InvalidInput("Name needs to be between 1 and 255 characters long.");
                }

                if (!Validation.IsValidEmailAddress(email_address))
                {
                    throw new Domain.Exceptions.InvalidInput("Profile email is not a valid email address.");
                }

                if (!isEmailAddressAvailable(email_address, user_id))
                {
                    throw new Domain.Exceptions.InvalidInput("The email address is already associated to another user.");
                }

                #endregion

                var linq_getter = new LinqGetter(context);
                var user = linq_getter.GetUser(user_id);
                var login = linq_getter.GetLogin(user_id);

                user.Name = name;
                user.EmailAddress = email_address;
                login.TimeZone = timezone;

                context.SubmitChanges();

                // Remove this user from memcached...
                X2O.Utilities.CachedInfo.RemovePersonWithEmail(user.ID);
                X2O.Utilities.CachedInfo.RemovePersonBase(user.ID);
            }
        }
        ```
        
        
