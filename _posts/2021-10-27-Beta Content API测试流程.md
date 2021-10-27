---
layout: post
title:  "Beta Content API测试流程"
date:   2021-10-27 00:15:56 +0800
categories: [工作]
---

1. 用Postman从http://portal.mintel.com/portal/api/contentToken拿到jwt token。  
GET  
Header Key设置为“Authorization”，Value设置为“Bearer be25bfb1-c72c-4aa7-b8ac-c3780e28c0d2” （test-ted的token）   
  
2. 再从https://beta-api.mintel.com/graphql/v1/发request  
POST  
Header Key设置为“Authorization”，Value设置为“Bearer 刚才拿到的token” 。  
Body Key 设置为“query”， Value为Query Pipeline， 比如：  
```
query testQuery($query: ContentSearchQueryInput, $paginationSettings: PaginationInput! , $accessGroups: AccessGroupInput) {
  contentSearch(query: $query, paginationSettings: $paginationSettings, accessGroups: $accessGroups) {
    totalCount
    pageInfo {
      hasNextPage
      startCursor
      endCursor
    }
    results {
      access {
        id
      }
      status
      item {
        ... on ContentItem {
          id
          attachments {
            fileName
            type {
              mimeType
            }
            url
          }
          author {
            name
          }
          contentText
          dateOfLastModification {
            unixTimestamp
          }
          dateToDisplay {
            unixTimestamp
          }
          summaryText
          tags {
            ... on Category {
              id
              name
            }
            ... on Region {
              id
              name
            }
          }
          title
          url
        }
        ... on DeletedContentItem {
          id
          dateOfDeletion {
            unixTimestamp
          }
        }
      }
    }
  }
}
```
再加一个Body Key为variables，Values设置为请求参数，比如：  
```
{"query": {"freetext": "car"},  "sortOrder": "RELEVANT", "paginationSettings": {"after": "*", "first": 1}, "accessGroups": {"access": [29437]}}
``` 