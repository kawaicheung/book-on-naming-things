# Flow

When we name a variable, we need to look at how that variable is actually being used. Sometimes, a variable name in isolation feels right, but when it's used, a key piece of information is missing. Here's a simple example.

I have some code inside a larger method that dictates whether someone should receive an email notification.

```C#

if (user.IsOwner ...)
{
	sendNotification(user);
}

```


 private List<int> getProjectIDsForUserInAccount(List<int> project_ids, VerifiedRequester requester)
        {
            var valid_projects = requester.AccessibleProjects;

            List<int> valid_project_ids = valid_projects.Select(p => p.ID).ToList();

            if (only_include_these_project_ids != null && only_include_these_project_ids.Any())
            {
                only_include_these_project_ids = only_include_these_project_ids.Where(p => valid_project_ids.Contains(p)).ToList();
            }

            if (only_include_these_project_ids == null || only_include_these_project_ids.Count == 0)
            {
                only_include_these_project_ids = valid_project_ids;
            }

            return only_include_these_project_ids;
        }


        TO...  (project_id_candidates...as an attempt), then FLIPPING params... so requester is first.

  private List<int> filterProjectIDsRequesterCanAccess(List<int> only_these_project_ids, VerifiedRequester requester)
        {
            List<int> valid_project_ids = requester.AccessibleProjectIDs;

            if (only_these_project_ids == null || !only_these_project_ids.Any())
            {
                return valid_project_ids;
            }

            return only_these_project_ids.Where(p => valid_project_ids.Contains(p)).ToList();
        }


          private List<int> filterProjectIDsRequesterCanAccess(VerifiedRequester requester, List<int> only_these_project_ids)
        {
            List<int> valid_project_ids = requester.AccessibleProjectIDs;

            if (only_these_project_ids == null || !only_these_project_ids.Any())
            {
                return valid_project_ids;
            }

            return only_these_project_ids.Where(p => valid_project_ids.Contains(p)).ToList();
        }
