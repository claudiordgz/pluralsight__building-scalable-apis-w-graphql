# Query

```
query TestQuery {
  graphQLHub
  github {
    user(username: "claudiordgz") {
      id
      company
      avatar_url
      repos {
        name
      }
    }
  }
}
```

# Fields

```
query TestQuery {
  graphQLHub <- String
  github { <- Complex Field
    user(username: "claudiordgz") { <- Complex Field
      id <- Int
      company <- String
      avatar_url <- String
      repos { <- Complex Field
        name <- String
      }
    }
  }
}
```


# Variables

```
query TestQuery($currentUserName: String!) {
  graphQLHub
  github {
    user(username: $currentUserName) {
      id
      company
      avatar_url
      repos {
        name
      }
    }
  }
}
```

Then in the query Variables

```
{
  "currentUserName" : "claudiordgz"
}
```

# Directives

## Include if some flag Directive 

```
query TestQuery(
  $currentUserName: String!
  $includeRepos: Boolean!
) {
  graphQLHub
  github {
    user(username: $currentUserName) {
      id
      company
      avatar_url
      repos @include(if: $includeRepos) {
        name
      }
    }
  }
}
```

## Same thing but with Skip


```
query TestQuery(
  $currentUserName: String!
  $excludeRepos: Boolean!
) {
  graphQLHub
  github {
    user(username: $currentUserName) {
      id
      company
      avatar_url
      repos @skip(if: $excludeRepos) {
        name
      }
    }
  }
}
```

```
{
  "currentUserName" : "claudiordgz",
  "includeRepos": false
}
```

OR

```
{
  "currentUserName" : "claudiordgz",
  "excludeRepos": true
}
```

# Aliases

On ocassions some field may come with a name different from what our template needs. For example in this case comes as `id` but we may need `the_id`. This is where aliases come in handy, for we can rename the field on the query.

```
query TestQuery(
  $currentUserName: String!
) {
  github {
    user(username: $currentUserName) {
      the_id: id
      company
      avatar_url
    }
  }
}
```

## Getting tuples with the help of variables and aliases

Let's say we want to get 2 users from our service

```
query TestQuery(
  $user1: String!
  $user2: String!
) {
  github {
    user1: user(username: $user1) {
      id
      company
      avatar_url
    }
    user2: user(username: $user1) {
      id
      company
      avatar_url
    }
  }
}
```

And to retrieve it

```
{
  "user1": "claudiordgz"
  "user2": "cthulhu"
}
```

# Fragments

To compose a query of stuff instead of repeat things:

```
query TestQuery(
  $userName1: String!
  $userName2: String!
) {
  github {
    user1 : user(username: $userName1) {
      ... UserInfo 
    }
    user2 : user(username: $userName2) {
      ... UserInfo
    }
  }
}
    
fragment UserInfo on GithubUser {
  id
  company
  avatar_url
}
```

And call it the same way as before. Notice `GithubUser` which is defined on the Github spec.

# Inline Fragments



```
{
  github {
    repo(name:"graphql", ownerUsername: "facebook") {
      commits {
        message
        author {
          ... on GithubUser {
            login
          }
          ... on GithubCommitAuthor {
            email
          }
        }
      }
    }
  }
}
```
