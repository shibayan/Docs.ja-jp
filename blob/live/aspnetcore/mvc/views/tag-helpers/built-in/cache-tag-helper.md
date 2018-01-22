---
title: "ASP.NET Core MVC でのタグ ヘルパーをキャッシュします。"
author: pkellner
description: "キャッシュ タグ ヘルパーを使用する方法を示しています。"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: dfd9c3c0c4e50a99e4f8703b01bd9b384930b87a
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a><span data-ttu-id="74e48-103">ASP.NET Core MVC でのタグ ヘルパーをキャッシュします。</span><span class="sxs-lookup"><span data-stu-id="74e48-103">Cache Tag Helper in ASP.NET Core MVC</span></span>

<span data-ttu-id="74e48-104">著者: [Peter Kellner](http://peterkellner.net)</span><span class="sxs-lookup"><span data-stu-id="74e48-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="74e48-105">キャッシュ タグ ヘルパーでは、内部の ASP.NET Core キャッシュ プロバイダーには、そのコンテンツをキャッシュすることによって、ASP.NET Core アプリケーションのパフォーマンスを大幅に改善する機能を提供します。</span><span class="sxs-lookup"><span data-stu-id="74e48-105">The Cache Tag Helper provides the ability to dramatically improve the performance of your ASP.NET Core app by caching its content to the internal ASP.NET Core cache provider.</span></span>

<span data-ttu-id="74e48-106">Razor ビュー エンジンが既定値を設定`expires-after`20 分にします。</span><span class="sxs-lookup"><span data-stu-id="74e48-106">The Razor View Engine sets the default `expires-after` to twenty minutes.</span></span>

<span data-ttu-id="74e48-107">次の Razor マークアップでは、日付/時刻によってキャッシュされます。</span><span class="sxs-lookup"><span data-stu-id="74e48-107">The following Razor markup caches the date/time:</span></span>

```cshtml
<cache>@DateTime.Now</cache>
```

<span data-ttu-id="74e48-108">最初の要求を含むページ`CacheTagHelper`現在の日付/時刻が表示されます。</span><span class="sxs-lookup"><span data-stu-id="74e48-108">The first request to the page that contains `CacheTagHelper` will display the current date/time.</span></span> <span data-ttu-id="74e48-109">キャッシュ (既定値は 20 分間) 有効期限が切れるか、メモリ不足によって削除されるまで、追加の要求は、キャッシュされた値を表示します。</span><span class="sxs-lookup"><span data-stu-id="74e48-109">Additional requests will show the cached value until the cache expires (default 20 minutes) or is evicted by memory pressure.</span></span>

<span data-ttu-id="74e48-110">次の属性を持つキャッシュの存続期間を設定できます。</span><span class="sxs-lookup"><span data-stu-id="74e48-110">You can set the cache duration with the following attributes:</span></span>

## <a name="cache-tag-helper-attributes"></a><span data-ttu-id="74e48-111">キャッシュ タグ ヘルパー属性</span><span class="sxs-lookup"><span data-stu-id="74e48-111">Cache Tag Helper Attributes</span></span>

- - -

### <a name="enabled"></a><span data-ttu-id="74e48-112">enabled</span><span class="sxs-lookup"><span data-stu-id="74e48-112">enabled</span></span>    


| <span data-ttu-id="74e48-113">属性の型</span><span class="sxs-lookup"><span data-stu-id="74e48-113">Attribute Type</span></span>    | <span data-ttu-id="74e48-114">有効な値</span><span class="sxs-lookup"><span data-stu-id="74e48-114">Valid Values</span></span>      |
|----------------   |----------------   |
| <span data-ttu-id="74e48-115">boolean</span><span class="sxs-lookup"><span data-stu-id="74e48-115">boolean</span></span>           | <span data-ttu-id="74e48-116">"true"(既定値)</span><span class="sxs-lookup"><span data-stu-id="74e48-116">"true" (default)</span></span>  |
|                   | <span data-ttu-id="74e48-117">"false"</span><span class="sxs-lookup"><span data-stu-id="74e48-117">"false"</span></span>   |


<span data-ttu-id="74e48-118">キャッシュ タグ ヘルパーで囲まれたコンテンツをキャッシュするかどうかを判断します。</span><span class="sxs-lookup"><span data-stu-id="74e48-118">Determines whether the content enclosed by the Cache Tag Helper is cached.</span></span> <span data-ttu-id="74e48-119">既定値は、`true` です。</span><span class="sxs-lookup"><span data-stu-id="74e48-119">The default is `true`.</span></span>  <span data-ttu-id="74e48-120">場合設定`false`このキャッシュ タグ ヘルパーには、表示される出力のキャッシュの効果はありません。</span><span class="sxs-lookup"><span data-stu-id="74e48-120">If set to `false` this Cache Tag Helper will have no caching effect on the rendered output.</span></span>

<span data-ttu-id="74e48-121">例:</span><span class="sxs-lookup"><span data-stu-id="74e48-121">Example:</span></span>

```cshtml
<cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-on"></a><span data-ttu-id="74e48-122">expires-on</span><span class="sxs-lookup"><span data-stu-id="74e48-122">expires-on</span></span> 

| <span data-ttu-id="74e48-123">属性の型</span><span class="sxs-lookup"><span data-stu-id="74e48-123">Attribute Type</span></span>    | <span data-ttu-id="74e48-124">値の例</span><span class="sxs-lookup"><span data-stu-id="74e48-124">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="74e48-125">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="74e48-125">DateTimeOffset</span></span>    | <span data-ttu-id="74e48-126">"@new DateTime(2025,1,29,17,02,0)"</span><span class="sxs-lookup"><span data-stu-id="74e48-126">"@new DateTime(2025,1,29,17,02,0)"</span></span>    |


<span data-ttu-id="74e48-127">絶対有効期限を設定します。</span><span class="sxs-lookup"><span data-stu-id="74e48-127">Sets an absolute expiration date.</span></span> <span data-ttu-id="74e48-128">次の例は、キャッシュ タグ ヘルパーの内容を 2025 年 1 月 29日の 5時 02分 PM までキャッシュされます。</span><span class="sxs-lookup"><span data-stu-id="74e48-128">The following example will cache the contents of the Cache Tag Helper until 5:02 PM on January 29, 2025.</span></span>

<span data-ttu-id="74e48-129">例:</span><span class="sxs-lookup"><span data-stu-id="74e48-129">Example:</span></span>

```cshtml
<cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-after"></a><span data-ttu-id="74e48-130">expires-after</span><span class="sxs-lookup"><span data-stu-id="74e48-130">expires-after</span></span>

| <span data-ttu-id="74e48-131">属性の型</span><span class="sxs-lookup"><span data-stu-id="74e48-131">Attribute Type</span></span>    | <span data-ttu-id="74e48-132">値の例</span><span class="sxs-lookup"><span data-stu-id="74e48-132">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="74e48-133">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="74e48-133">TimeSpan</span></span>    | <span data-ttu-id="74e48-134">"@TimeSpan.FromSeconds(120)"</span><span class="sxs-lookup"><span data-stu-id="74e48-134">"@TimeSpan.FromSeconds(120)"</span></span>    |


<span data-ttu-id="74e48-135">内容をキャッシュする最初の要求時から時間の長さを設定します。</span><span class="sxs-lookup"><span data-stu-id="74e48-135">Sets the length of time from the first request time to cache the contents.</span></span> 

<span data-ttu-id="74e48-136">例:</span><span class="sxs-lookup"><span data-stu-id="74e48-136">Example:</span></span>

```cshtml
<cache expires-after="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="expires-sliding"></a><span data-ttu-id="74e48-137">expires-sliding</span><span class="sxs-lookup"><span data-stu-id="74e48-137">expires-sliding</span></span>

| <span data-ttu-id="74e48-138">属性の型</span><span class="sxs-lookup"><span data-stu-id="74e48-138">Attribute Type</span></span>    | <span data-ttu-id="74e48-139">値の例</span><span class="sxs-lookup"><span data-stu-id="74e48-139">Example Value</span></span>     |
|----------------   |----------------   |
| <span data-ttu-id="74e48-140">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="74e48-140">TimeSpan</span></span>    | <span data-ttu-id="74e48-141">"@TimeSpan.FromSeconds(60)"</span><span class="sxs-lookup"><span data-stu-id="74e48-141">"@TimeSpan.FromSeconds(60)"</span></span>     |


<span data-ttu-id="74e48-142">アクセスされていない場合、キャッシュ エントリを削除する必要がありますを時間を設定します。</span><span class="sxs-lookup"><span data-stu-id="74e48-142">Sets the time that a cache entry should be evicted if it has not been accessed.</span></span>

<span data-ttu-id="74e48-143">例:</span><span class="sxs-lookup"><span data-stu-id="74e48-143">Example:</span></span>

```cshtml
<cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-header"></a><span data-ttu-id="74e48-144">vary-by-header</span><span class="sxs-lookup"><span data-stu-id="74e48-144">vary-by-header</span></span>

| <span data-ttu-id="74e48-145">属性の型</span><span class="sxs-lookup"><span data-stu-id="74e48-145">Attribute Type</span></span>    | <span data-ttu-id="74e48-146">値の例</span><span class="sxs-lookup"><span data-stu-id="74e48-146">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="74e48-147">String</span><span class="sxs-lookup"><span data-stu-id="74e48-147">String</span></span>            | <span data-ttu-id="74e48-148">"User-Agent"</span><span class="sxs-lookup"><span data-stu-id="74e48-148">"User-Agent"</span></span>                  |
|                   | <span data-ttu-id="74e48-149">"User-Agent,content-encoding"</span><span class="sxs-lookup"><span data-stu-id="74e48-149">"User-Agent,content-encoding"</span></span> |

<span data-ttu-id="74e48-150">1 つのヘッダー値または変更されるときに、キャッシュの更新をトリガーするヘッダー値のコンマ区切りの一覧を受け入れます。</span><span class="sxs-lookup"><span data-stu-id="74e48-150">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when they change.</span></span> <span data-ttu-id="74e48-151">次の例は、ヘッダーの値を監視`User-Agent`です。</span><span class="sxs-lookup"><span data-stu-id="74e48-151">The following example monitors the header value `User-Agent`.</span></span> <span data-ttu-id="74e48-152">コンテンツをキャッシュする例では、すべて異なる`User-Agent`web サーバーに提示します。</span><span class="sxs-lookup"><span data-stu-id="74e48-152">The example will cache the content for every different `User-Agent` presented to the web server.</span></span>

<span data-ttu-id="74e48-153">例:</span><span class="sxs-lookup"><span data-stu-id="74e48-153">Example:</span></span>

```cshtml
<cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-query"></a><span data-ttu-id="74e48-154">vary-by-query</span><span class="sxs-lookup"><span data-stu-id="74e48-154">vary-by-query</span></span>

| <span data-ttu-id="74e48-155">属性の型</span><span class="sxs-lookup"><span data-stu-id="74e48-155">Attribute Type</span></span>    | <span data-ttu-id="74e48-156">値の例</span><span class="sxs-lookup"><span data-stu-id="74e48-156">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="74e48-157">String</span><span class="sxs-lookup"><span data-stu-id="74e48-157">String</span></span>            | <span data-ttu-id="74e48-158">「処理を行う」</span><span class="sxs-lookup"><span data-stu-id="74e48-158">"Make"</span></span>                |
|                   | <span data-ttu-id="74e48-159">「製造元、モデル」</span><span class="sxs-lookup"><span data-stu-id="74e48-159">"Make,Model"</span></span> |

<span data-ttu-id="74e48-160">1 つのヘッダー値またはヘッダーの値が変化したときに、キャッシュの更新をトリガーするヘッダー値のコンマ区切りの一覧を受け入れます。</span><span class="sxs-lookup"><span data-stu-id="74e48-160">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header value changes.</span></span> <span data-ttu-id="74e48-161">次の例の値を調べ`Make`と`Model`です。</span><span class="sxs-lookup"><span data-stu-id="74e48-161">The following example looks at the values of `Make` and `Model`.</span></span>

<span data-ttu-id="74e48-162">例:</span><span class="sxs-lookup"><span data-stu-id="74e48-162">Example:</span></span>

```cshtml
<cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-route"></a><span data-ttu-id="74e48-163">vary-by-route</span><span class="sxs-lookup"><span data-stu-id="74e48-163">vary-by-route</span></span>

| <span data-ttu-id="74e48-164">属性の型</span><span class="sxs-lookup"><span data-stu-id="74e48-164">Attribute Type</span></span>    | <span data-ttu-id="74e48-165">値の例</span><span class="sxs-lookup"><span data-stu-id="74e48-165">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="74e48-166">String</span><span class="sxs-lookup"><span data-stu-id="74e48-166">String</span></span>            | <span data-ttu-id="74e48-167">「処理を行う」</span><span class="sxs-lookup"><span data-stu-id="74e48-167">"Make"</span></span>                |
|                   | <span data-ttu-id="74e48-168">「製造元、モデル」</span><span class="sxs-lookup"><span data-stu-id="74e48-168">"Make,Model"</span></span> |

<span data-ttu-id="74e48-169">1 つのヘッダー値またはルート データ パラメーターの値変更時に、キャッシュの更新をトリガーするヘッダー値のコンマ区切りの一覧を受け入れます。</span><span class="sxs-lookup"><span data-stu-id="74e48-169">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the route data parameter value(s) change.</span></span> <span data-ttu-id="74e48-170">例:</span><span class="sxs-lookup"><span data-stu-id="74e48-170">Example:</span></span>

<span data-ttu-id="74e48-171">*Startup.cs*</span><span class="sxs-lookup"><span data-stu-id="74e48-171">*Startup.cs*</span></span> 

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```
  
<span data-ttu-id="74e48-172">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="74e48-172">*Index.cshtml*</span></span>

```cshtml
<cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-cookie"></a><span data-ttu-id="74e48-173">vary-by-cookie</span><span class="sxs-lookup"><span data-stu-id="74e48-173">vary-by-cookie</span></span>

| <span data-ttu-id="74e48-174">属性の型</span><span class="sxs-lookup"><span data-stu-id="74e48-174">Attribute Type</span></span>    | <span data-ttu-id="74e48-175">値の例</span><span class="sxs-lookup"><span data-stu-id="74e48-175">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="74e48-176">String</span><span class="sxs-lookup"><span data-stu-id="74e48-176">String</span></span>            | <span data-ttu-id="74e48-177">".AspNetCore.Identity.Application"</span><span class="sxs-lookup"><span data-stu-id="74e48-177">".AspNetCore.Identity.Application"</span></span>                |
|                   | <span data-ttu-id="74e48-178">".AspNetCore.Identity.Application,HairColor"</span><span class="sxs-lookup"><span data-stu-id="74e48-178">".AspNetCore.Identity.Application,HairColor"</span></span> |

<span data-ttu-id="74e48-179">1 つのヘッダー値またはヘッダーの値 (秒) の変更時に、キャッシュの更新をトリガーするヘッダー値のコンマ区切りの一覧を受け入れます。</span><span class="sxs-lookup"><span data-stu-id="74e48-179">Accepts a single header value or a comma-separated list of header values that trigger a cache refresh when the header values(s) change.</span></span> <span data-ttu-id="74e48-180">次の例は、ASP.NET の Id に関連付けられているクッキーを検索します。</span><span class="sxs-lookup"><span data-stu-id="74e48-180">The following example looks at the cookie associated with ASP.NET Identity.</span></span> <span data-ttu-id="74e48-181">ユーザーが認証されるときに設定する要求 cookie キャッシュの更新をトリガーします。</span><span class="sxs-lookup"><span data-stu-id="74e48-181">When a user is authenticated the request cookie to be set which triggers a cache refresh.</span></span>

<span data-ttu-id="74e48-182">例:</span><span class="sxs-lookup"><span data-stu-id="74e48-182">Example:</span></span>

```cshtml
<cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="vary-by-user"></a><span data-ttu-id="74e48-183">vary-by-user</span><span class="sxs-lookup"><span data-stu-id="74e48-183">vary-by-user</span></span>

| <span data-ttu-id="74e48-184">属性の型</span><span class="sxs-lookup"><span data-stu-id="74e48-184">Attribute Type</span></span>    | <span data-ttu-id="74e48-185">値の例</span><span class="sxs-lookup"><span data-stu-id="74e48-185">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="74e48-186">ブール型</span><span class="sxs-lookup"><span data-stu-id="74e48-186">Boolean</span></span>             | <span data-ttu-id="74e48-187">"true"</span><span class="sxs-lookup"><span data-stu-id="74e48-187">"true"</span></span>                  |
|                     | <span data-ttu-id="74e48-188">"false"(既定値)</span><span class="sxs-lookup"><span data-stu-id="74e48-188">"false" (default)</span></span> |

<span data-ttu-id="74e48-189">ログインしたユーザー (またはコンテキストのプリンシパル) が変更されたときにキャッシュをリセットするかどうかを指定します。</span><span class="sxs-lookup"><span data-stu-id="74e48-189">Specifies whether or not the cache should reset when the logged-in user (or Context Principal) changes.</span></span> <span data-ttu-id="74e48-190">現在のユーザー コンテキストの要求プリンシパルとも呼ばれます、Razor ビューで参照することで表示できます`@User.Identity.Name`です。</span><span class="sxs-lookup"><span data-stu-id="74e48-190">The current user is also known as the Request Context Principal and can be viewed in a Razor view by referencing `@User.Identity.Name`.</span></span>

<span data-ttu-id="74e48-191">次の例は、現在ログインしているユーザーで検索します。</span><span class="sxs-lookup"><span data-stu-id="74e48-191">The following example looks at the current logged in user.</span></span>  

<span data-ttu-id="74e48-192">例:</span><span class="sxs-lookup"><span data-stu-id="74e48-192">Example:</span></span>

```cshtml
<cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="74e48-193">この属性を使用してログインし、ログ アウト サイクルを使用してキャッシュの内容を保持します。</span><span class="sxs-lookup"><span data-stu-id="74e48-193">Using this attribute maintains the contents in cache through a log-in and log-out cycle.</span></span>  <span data-ttu-id="74e48-194">使用する場合`vary-by-user="true"`ログで、ログ アウト アクションが、認証されたユーザーのキャッシュを無効になります。</span><span class="sxs-lookup"><span data-stu-id="74e48-194">When using `vary-by-user="true"`, a log-in and log-out action invalidates the cache for the authenticated user.</span></span>  <span data-ttu-id="74e48-195">ログインの新しい一意の cookie の値が生成されるため、キャッシュは無効になります。</span><span class="sxs-lookup"><span data-stu-id="74e48-195">The cache is invalidated because a new unique cookie value is generated on login.</span></span> <span data-ttu-id="74e48-196">Cookie が存在するか有効期限が切れたときに、匿名の状態のキャッシュが保持されません。</span><span class="sxs-lookup"><span data-stu-id="74e48-196">Cache is maintained for the anonymous state when no cookie is present or has expired.</span></span> <span data-ttu-id="74e48-197">これユーザーがログインしていない場合、キャッシュを維持することを意味します。</span><span class="sxs-lookup"><span data-stu-id="74e48-197">This means if no user is logged in, the cache will be maintained.</span></span>

- - -

### <a name="vary-by"></a><span data-ttu-id="74e48-198">vary-by</span><span class="sxs-lookup"><span data-stu-id="74e48-198">vary-by</span></span>

| <span data-ttu-id="74e48-199">属性の型</span><span class="sxs-lookup"><span data-stu-id="74e48-199">Attribute Type</span></span>    | <span data-ttu-id="74e48-200">値の例</span><span class="sxs-lookup"><span data-stu-id="74e48-200">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="74e48-201">String</span><span class="sxs-lookup"><span data-stu-id="74e48-201">String</span></span>             | <span data-ttu-id="74e48-202">"@Model"</span><span class="sxs-lookup"><span data-stu-id="74e48-202">"@Model"</span></span>                 |


<span data-ttu-id="74e48-203">どのようなデータを取得キャッシュのカスタマイズをできます。</span><span class="sxs-lookup"><span data-stu-id="74e48-203">Allows for customization of what data gets cached.</span></span> <span data-ttu-id="74e48-204">属性の文字列値が変更された、キャッシュ タグ ヘルパーの内容によって参照されるオブジェクトが更新されたとき。</span><span class="sxs-lookup"><span data-stu-id="74e48-204">When the object referenced by the attribute's string value changes, the content of the Cache Tag Helper is updated.</span></span> <span data-ttu-id="74e48-205">多くの場合、モデルの値の文字列の連結は、この属性に割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="74e48-205">Often a string-concatenation of model values are assigned to this attribute.</span></span>  <span data-ttu-id="74e48-206">実際には、連結された値のいずれかの更新には、キャッシュが無効になります。 このためです。</span><span class="sxs-lookup"><span data-stu-id="74e48-206">Effectively, that means an update to any of the concatenated values invalidates the cache.</span></span>

<span data-ttu-id="74e48-207">次の例では、ビューの合計値に 2 つのルート パラメーターの整数値を表示するコント ローラー メソッド`myParam1`と`myParam2`、1 つのモデルのプロパティとしてを返します。</span><span class="sxs-lookup"><span data-stu-id="74e48-207">The following example assumes the controller method rendering the view sums the integer value of the two route parameters, `myParam1` and `myParam2`, and returns that as the single model property.</span></span> <span data-ttu-id="74e48-208">この合計が変更されたときに、キャッシュ タグ ヘルパーのコンテンツのレンダリングし、キャッシュがもう一度です。</span><span class="sxs-lookup"><span data-stu-id="74e48-208">When this sum changes, the content of the Cache Tag Helper is rendered and cached again.</span></span>  

<span data-ttu-id="74e48-209">例:</span><span class="sxs-lookup"><span data-stu-id="74e48-209">Example:</span></span>

<span data-ttu-id="74e48-210">アクション:</span><span class="sxs-lookup"><span data-stu-id="74e48-210">Action:</span></span>

```csharp
public IActionResult Index(string myParam1,string myParam2,string myParam3)
{
    int num1;
    int num2;
    int.TryParse(myParam1, out num1);
    int.TryParse(myParam2, out num2);
    return View(viewName, num1 + num2);
}
```

<span data-ttu-id="74e48-211">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="74e48-211">*Index.cshtml*</span></span>

```cshtml
<cache vary-by="@Model"">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

- - -

### <a name="priority"></a><span data-ttu-id="74e48-212">priority</span><span class="sxs-lookup"><span data-stu-id="74e48-212">priority</span></span>

| <span data-ttu-id="74e48-213">属性の型</span><span class="sxs-lookup"><span data-stu-id="74e48-213">Attribute Type</span></span>    | <span data-ttu-id="74e48-214">値の例</span><span class="sxs-lookup"><span data-stu-id="74e48-214">Example Values</span></span>                |
|----------------   |----------------               |
| <span data-ttu-id="74e48-215">CacheItemPriority</span><span class="sxs-lookup"><span data-stu-id="74e48-215">CacheItemPriority</span></span>  | <span data-ttu-id="74e48-216">「高」</span><span class="sxs-lookup"><span data-stu-id="74e48-216">"High"</span></span>                   |
|                    | <span data-ttu-id="74e48-217">「低」</span><span class="sxs-lookup"><span data-stu-id="74e48-217">"Low"</span></span> |
|                    | <span data-ttu-id="74e48-218">"NeverRemove"</span><span class="sxs-lookup"><span data-stu-id="74e48-218">"NeverRemove"</span></span> |
|                    | <span data-ttu-id="74e48-219">"Normal"</span><span class="sxs-lookup"><span data-stu-id="74e48-219">"Normal"</span></span> |

<span data-ttu-id="74e48-220">組み込みのキャッシュ プロバイダーをキャッシュ立ち退きのガイダンスを提供します。</span><span class="sxs-lookup"><span data-stu-id="74e48-220">Provides cache eviction guidance to the built-in cache provider.</span></span> <span data-ttu-id="74e48-221">Web サーバーを削除し`Low`メモリ不足である場合に、最初のエントリをキャッシュします。</span><span class="sxs-lookup"><span data-stu-id="74e48-221">The web server will evict `Low` cache entries first when it's under memory pressure.</span></span>

<span data-ttu-id="74e48-222">例:</span><span class="sxs-lookup"><span data-stu-id="74e48-222">Example:</span></span>

```cshtml
<cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="74e48-223">`priority`属性はキャッシュの保有期間の特定のレベルを保証されません。</span><span class="sxs-lookup"><span data-stu-id="74e48-223">The `priority` attribute does not guarantee a specific level of cache retention.</span></span> <span data-ttu-id="74e48-224">`CacheItemPriority`提案のみです。</span><span class="sxs-lookup"><span data-stu-id="74e48-224">`CacheItemPriority` is only a suggestion.</span></span> <span data-ttu-id="74e48-225">この属性を設定する`NeverRemove`キャッシュ常に保持することは保証されません。</span><span class="sxs-lookup"><span data-stu-id="74e48-225">Setting this attribute to `NeverRemove` does not guarantee that the cache will always be retained.</span></span> <span data-ttu-id="74e48-226">参照してください[その他のリソース](#additional-resources)詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="74e48-226">See [Additional Resources](#additional-resources) for more information.</span></span>

<span data-ttu-id="74e48-227">キャッシュ タグ ヘルパーが依存、[メモリ キャッシュ サービス](xref:performance/caching/memory)です。</span><span class="sxs-lookup"><span data-stu-id="74e48-227">The Cache Tag Helper is dependent on the [memory cache service](xref:performance/caching/memory).</span></span> <span data-ttu-id="74e48-228">キャッシュ タグ ヘルパーは、追加されていない場合に、サービスを追加します。</span><span class="sxs-lookup"><span data-stu-id="74e48-228">The Cache Tag Helper adds the service if it has not been added.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="74e48-229">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="74e48-229">Additional resources</span></span>

* <xref:performance/caching/memory>
* <xref:security/authentication/identity>