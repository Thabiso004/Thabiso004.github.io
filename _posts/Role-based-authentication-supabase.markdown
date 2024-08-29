---
layout: post
title:  "Role Based Authentication"
date:   2024-08-24 11:46 +0200
categories: databases
---
# 1. Defining Roles

For a more human-friendly approach to referencing roles, you can create a custom enum type:
```sql
CREATE TYPE user_role AS ENUM ('spots_moderator', 'admin');
```

# 2. Setting Up the “user_roles” Table

The `user_roles` table allows us to link roles with users. To ensure the security of user_roles, enable Row-Level Security (RLS) and implement the policy:

# 2. Setting Up the “user_roles” Table

The `user_roles` table allows us to link roles with users. To ensure the security of user_roles, enable Row-Level Security (RLS) and implement the policy:

![](https://miro.medium.com/v2/resize:fit:525/1*bgt9cwgv-PIP22xNMQ_m2g.png)

To secure `user_roles` enable RLS (Row-level security) and add following policy `auth.uid() = user_id`

![](https://miro.medium.com/v2/resize:fit:700/1*2TH1olH83xgtjYJ7Ad3Shw.png)

This policy grants authenticated users access to read the roles they possess.

Key points:

- `user_id` references `auth.users.id`
- The role is based on the `user_role` type (the enum type from step 1.)
- RLS enables reading of roles owned by the user

# 3. Implementing Row-Level Security for Role-Specific Access

You can now control access to specific rows within a table, such as `spot_proposals` in this case, by users with particular roles. This is done by specifying the following RLS condition:

![](https://miro.medium.com/v2/resize:fit:700/1*6TTjaHJURngSCNAumm_D7Q.png)

Key points:

- `user_roles` has an RLS policy, allowing authenticated users to read their roles
- The RLS policy on `spot_proposals` restricts access to specific roles, such as `spots_moderator`

This setup ensures a straightforward yet effective role-based access control mechanism in Supabase.

Looking for more Supabase content? Join me on Twitter and Medium for the latest updates, tutorials, and insights! Follow along to stay in the loop.
