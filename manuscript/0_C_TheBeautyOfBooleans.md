## The Beauty of Booleans

negative bools
also is vs. can --> isUpdateable vs. canUpdate (latter reads better in use because its not a question... if (user.CanUpdate)... means ok i can (user.IsUpdatable almost lends itself to having to say user.IsUpdateable == true)


Small example but can be confusing otherwise...
 
```
foreach (var status_to_upsert in statuses_to_upsert)
{
    if (isValidID(status_to_upsert.ID) && !workflow.AllStatuses.Any(s => s.ID == status_to_upsert.ID))
    {
        throw new Domain.Exceptions.InvalidInput(string.Format("The status {0} is not part of this workflow.", status_to_upsert.Name));
    }
}
 ```
  vs
 

```
foreach (var status_to_upsert in statuses_to_upsert)
{
    var status_is_update = isValidID(status_to_upsert.ID);
    var workflow_does_not_include_status = !workflow.AllStatuses.Any(s => s.ID == status_to_upsert.ID);

    if (status_is_update && workflow_does_not_include_status)
    {
        throw new Domain.Exceptions.InvalidInput(string.Format("The status {0} is not part of this workflow.", status_to_upsert.Name));
    }
}
 ```
