Go to graphqlhub.com's playground and Fetch the commits from facebook's graphql repo, make sure to retrieve date and message of each commit.

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


query ListOfCommits {
  github {
      repo (name: "graphql", ownerUsername: "facebook") {
          commits {
              date
              message
          }
      }
  }
}
