---
title: Using global node IDs
intro: You can get global node IDs of objects via the REST API and use them in GraphQL operations.
redirect_from:
  - /v4/guides/using-global-node-ids
versions:
  fpt: '*'
  ghec: '*'
  ghes: '*'
topics:
  - API
---

You can access most objects in GitHub (users, issues, pull requests, etc.) using either the REST API or the GraphQL API. You can find the **global node ID** of many objects from within the REST API and use these IDs in your GraphQL operations. For more information, see "[Preview GraphQL API Node IDs in REST API resources](https://developer.github.com/changes/2017-12-19-graphql-node-id/)."

{% note %}

**Note:** In REST, the global node ID field is named `node_id`. In GraphQL, it's an `id` field on the `node` interface. For a refresher on what "node" means in GraphQL, see "[AUTOTITLE](/graphql/guides/introduction-to-graphql#node)."

{% endnote%}

## وضع معرفات العقدة العالمية للاستخدام

يمكنك اتباع ثلاث خطوات لاستخدام معرفات العقدة العالمية بشكل فعال:

1. اتصل بنقطة نهاية REST التي تعيد كائنًا ما `node_id`.
1. Find the object's type in GraphQL.
1. Use the ID and type to do a direct node lookup in GraphQL.

Let's walk through an example.

## 1. Call a REST endpoint that returns an object's node ID

If you [request the authenticated user](/rest/users/users#get-the-authenticated-user):

```shell
curl -i --header "Authorization: Bearer YOUR-TOKEN" {% data variables.product.rest_url %}/user
```

you'll get a response that includes the `node_id` of the authenticated user:

```json
{
  "login": "octocat",
  "id": 1,
  "avatar_url": "https://github.com/images/error/octocat_happy.gif",
  "gravatar_id": "",
  "url": "https://api.github.com/users/octocat",
  "html_url": "https://github.com/octocat",
  "followers_url": "https://api.github.com/users/octocat/followers",
  "following_url": "https://api.github.com/users/octocat/following{/other_user}",
  "gists_url": "https://api.github.com/users/octocat/gists{/gist_id}",
  "starred_url": "https://api.github.com/users/octocat/starred{/owner}{/repo}",
  "subscriptions_url": "https://api.github.com/users/octocat/subscriptions",
  "organizations_url": "https://api.github.com/users/octocat/orgs",
  "repos_url": "https://api.github.com/users/octocat/repos",
  "events_url": "https://api.github.com/users/octocat/events{/privacy}",
  "received_events_url": "https://api.github.com/users/octocat/received_events",
  "type": "User",
  "site_admin": false,
  "name": "monalisa octocat",
  "company": "GitHub",
  "blog": "https://github.com/blog",
  "location": "San Francisco",
  "email": "octocat@github.com",
  "hireable": false,
  "bio": "There once was...",
  "public_repos": 2,
  "public_gists": 1,
  "followers": 20,
  "following": 0,
  "created_at": "2008-01-14T04:33:35Z",
  "updated_at": "2008-01-14T04:33:35Z",
  "private_gists": 81,
  "total_private_repos": 100,
  "owned_private_repos": 100,
  "disk_usage": 10000,
  "collaborators": 8,
  "two_factor_authentication": true,
  "plan": {
    "name": "Medium",
    "space": 400,
    "private_repos": 20,
    "collaborators": 0
  },
  "node_id": "MDQ6VXNlcjU4MzIzMQ=="
}
```

## 2. Find the object type in GraphQL

وفي هذا المثال، فإن 

ستحتاج إلى معرفة الكائن. _اكتب._ أولاً، رغم ذلك. يمكنك التحقق من النوع باستخدام استعلام GraphQL بسيط:

"'جرافيك.
الاستعلام {
 العقدة (المعرف: "MDQ6VXNlcU4MzIzMQ=") { 
 __typename. 
 } 
}
"'

هذا النوع من الاستعلام و MDash ؛ أي العثور على العقدة بواسطة ID&mdash ؛ يُعرف باسم "البحث المباشر عن العقدة".

عند تشغيل هذا الاستعلام.، سترى أن '__typename' هو. ["المستخدم"] (/graphql/reference/objects# المستخدم).

## 3. قم بالبحث المباشر عن العقدة في GraphQL

المستخدم.

"'الرسم.
الاستعلام {
 العقدة (المعرف: "MDQ6VXNlcU4MzIzMQ=") { 
 ... على المستخدم { 
 اسم. 
 تسجيل الدخول. 
 } 
 } 
}
"'

هذا النوع من الاستعلام هو النهج القياسي للبحث عن كائن بواسطة معرف العقدة العالمي.

## استخدام معرفات العقدة العالمية في الهجرات

عند بناء عمليات الدمج التي تستخدم إما واجهة برمجة تطبيقات REST أو واجهة برمجة تطبيقات GraphQL، من الأفضل الاستمرار في معرف العقدة العالمي حتى تتمكن من الرجوع بسهولة إلى الكائنات عبر إصدارات واجهة برمجة التطبيقات. لمزيد من المعلومات حول التعامل مع الانتقال بين REST و GraphQL، راجع "[مقالة][مقالة][مقالة][مقالة][مقالة][مقالة][مقالة][مقالة] (/graphql/guides/migrating-from-rest-to-graphql) ".
