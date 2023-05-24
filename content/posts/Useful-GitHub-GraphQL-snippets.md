---
title: 'Useful GitHub GraphQL Snippets'
date: 2023-02-13T13:43:27-05:00
draft: false
toc: true
cover:
tags:
  - github
  - graphql
---

I've been building a lot of GitHub automation recently and I've found myself using the same GraphQL queries over and over again. I thought it would be useful to share some of the most useful ones here.

## Queries

### Repos

#### Get the first 100 repositories in an organization

This query will return a list of all repositories in an organization. It will return the name, description, and URL of each repository along with pagination information.

```json
query {
	organization(login: "my-org") {
		repositories(first: 100) {
			pageInfo {
				hasNextPage
				endCursor
			}
			nodes {
				name
				description
				url
			}
		}
	}
}
```

#### Get the next 100 repositories in an organization with pagination

This query will return the next 100 repositories in an organization. It will return the name, description, and URL of each repository along with pagination information. Replace `$endCursor` with the value of `endCursor` from the previous query.

```json
query {
	organization(login: "my-org") {
		repositories(first: 100, after: $endCursor) {
			pageInfo {
				hasNextPage
				endCursor
			}
			nodes {
				name
				description
				url
			}
		}
	}
}
```

#### Get the first 100 repositories in an organization with a topic filter

This query will return a list of the first 100 repositories in an organization that have a specific topic. It will return the name, description, and URL of each repository along with pagination information.

```json
query {
	organization(login: "my-org") {
		repositories(first: 100, query: "topic:my-topic") {
			pageInfo {
				hasNextPage
				endCursor
			}
			nodes {
				name
				description
				url
			}
		}
	}
}
```

This query retrieves the existing branches in a repository.

```json
query getExistingRepoBranches{
  organization(login: "my-org") {
    repository(name: "my-repo") {
      id
      name
      refs(refPrefix: "refs/heads/", first: 10) {
        edges {
          node {¬
            branchName:name
          }
        }
      }
    }
  }
}
```

### Teams

This query fetches information about a specific team within an organization.

```json
query getTeam {
	organization(login: "my-org") {
		team(slug: "my-team") {
			id
		}
	}
}
```
This query retrieves information about teams within an organization, including the names of the teams and their members.

```json
query getTeams {
	organization(login: "my-org") {
		teams(first: 10) {
			edges {
				node {
					name
					members(first: 100) {
						nodes {
							name
						}
					}
				}
			}
		}
	}
}
```

This query retrieves repositories associated with a specific team, including the repository name and permission level.

```jsonŒ
query get_team_repos {
	organization(login: "my-org") {
		team(slug: "my-team") {
			name
			repositories(first: 3) {
				edges {
					permission
					node {
						name
					}
				}
			}
		}
	}
}
```

### Org

This query fetches the IP allow list entries for an organization, including details like ID, name, allow list value, and status.

```json
query get_IP_allow_list {
	organization(login: "my-org") {
		ipAllowListEntries(first: 100) {
			edges {
				node {
					id
					name
					allowListValue
					isActive
				}
			}
		}
	}
	organization(login: "my-org") {
		id
	}
}
```

### Enterprise

This query retrieves statistics for an enterprise, including billing information and member count.

```json
query getStats {
	enterprise(slug: "my-enterprise") {
		billingInfo {
			allLicensableUsersCount
			totalAvailableLicenses
			totalLicenses
			storageUsagePercentage
			bandwidthUsagePercentage
		}
		members {
			totalCount
		}
	}
	search(first: 100, type: REPOSITORY, query: "org:my-org") {
		repositoryCount
	}
}
```

## Mutations

### Repos

This mutation updates the settings of a repository, such as its description and wiki enablement.

```json
mutation change_repo_settings {
	updateRepository(
		input: {
			repositoryId: "my-repo-id"
			description: "updated via graphQL"
			hasWikiEnabled: true
		}
	) {
		repository {
			description
		}
	}
}
```

This mutation creates a branch protection rule for a repository.

```json
mutation create_branch_protection_rule {
	createBranchProtectionRule(
		input: {
			repositoryId: "my-repo-id"
			pattern: "production"
			allowsDeletions: true
		}
	) {
		branchProtectionRule {
			pattern
			allowsDeletions
		}
	}
}
```
This mutation updates an existing branch protection rule.

```json
mutation update_branch_protection_rule {
	updateBranchProtectionRule(
		input: {
			branchProtectionRuleId: "my-branch-protection-rule-id"
			allowsDeletions: false
			allowsForcePushes: false
			blocksCreations: true
			isAdminEnforced: true
			dismissesStaleReviews: true
			requiresCodeOwnerReviews: true
			requiredApprovingReviewCount: 1
			restrictsPushes: true
			requiresConversationResolution: true
			bypassForcePushActorIds: ["my-user-id"]
		}
	) {
		branchProtectionRule {
			allowsDeletions
		}
	}
}
```

This mutation sets a topic (tag) for a repository.
```json
mutation set_topic {
	updateTopics(input: { repositoryId: "my-repo-id", topicNames: "my-topic" }) {
		clientMutationId
	}
}
```

### Organizations

This mutation adds an IP to an organization's IP allow list.
```json
mutation add_ip_to_list {
	createIpAllowListEntry(
		input: {
			ownerId: "my-owner-id"
			allowListValue: "1.1.1.1"
			isActive: false
			name: "test"
		}
	) {
		ipAllowListEntry {
			name
		}
	}
}
```
