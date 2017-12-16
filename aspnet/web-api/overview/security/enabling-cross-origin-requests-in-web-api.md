---
uid: web-api/overview/security/enabling-cross-origin-requests-in-web-api
title: "ASP.NET Web API 2 でのクロス オリジン要求の有効化 |Microsoft ドキュメント"
author: MikeWasson
description: "ASP.NET Web API でクロス オリジン リソース共有 (CORS) をサポートする方法を示します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/15/2014
ms.topic: article
ms.assetid: 9b265a5a-6a70-4a82-adce-2d7c56ae8bdd
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/enabling-cross-origin-requests-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 453ad29ff4f10f9660f3aa8bab358519b4cfd48b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="enabling-cross-origin-requests-in-aspnet-web-api-2"></a><span data-ttu-id="16094-103">ASP.NET Web API 2 でのクロス オリジン要求を有効にします。</span><span class="sxs-lookup"><span data-stu-id="16094-103">Enabling Cross-Origin Requests in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="16094-104">によって[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="16094-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="16094-105">ブラウザーのセキュリティは、web ページが別のドメインに AJAX 要求を行うことを防止します。</span><span class="sxs-lookup"><span data-stu-id="16094-105">Browser security prevents a web page from making AJAX requests to another domain.</span></span> <span data-ttu-id="16094-106">この制限が呼び出された、*同一生成元ポリシー*、悪意のあるサイトが別のサイトから機密データを読み取ることを防ぎます。</span><span class="sxs-lookup"><span data-stu-id="16094-106">This restriction is called the *same-origin policy*, and prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="16094-107">ただし、場合もあります可能性がある、web API を呼び出す他のサイトを使用できます。</span><span class="sxs-lookup"><span data-stu-id="16094-107">However, sometimes you might want to let other sites call your web API.</span></span>
> 
> <span data-ttu-id="16094-108">[クロス オリジン リソース共有](http://www.w3.org/TR/cors/)(CORS) は、W3C 標準により、同じオリジンのポリシーを緩和するサーバーです。</span><span class="sxs-lookup"><span data-stu-id="16094-108">[Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="16094-109">CORS を使用して、サーバー明示的に許可できますいくつかのクロス オリジン要求中に、他のユーザーを拒否します。</span><span class="sxs-lookup"><span data-stu-id="16094-109">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="16094-110">CORS などがより安全なと以前の手法より柔軟な[JSONP](http://en.wikipedia.org/wiki/JSONP)です。</span><span class="sxs-lookup"><span data-stu-id="16094-110">CORS is safer and more flexible than earlier techniques such as [JSONP](http://en.wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="16094-111">このチュートリアルでは、Web API アプリケーションで CORS を有効にする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="16094-111">This tutorial shows how to enable CORS in your Web API application.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="16094-112">このチュートリアルで使用されているソフトウェアのバージョン</span><span class="sxs-lookup"><span data-stu-id="16094-112">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="16094-113">Visual Studio 2013 Update 2</span><span class="sxs-lookup"><span data-stu-id="16094-113">Visual Studio 2013 Update 2</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="16094-114">Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="16094-114">Web API 2.2</span></span>


<a id="intro"></a>
## <a name="introduction"></a><span data-ttu-id="16094-115">はじめに</span><span class="sxs-lookup"><span data-stu-id="16094-115">Introduction</span></span>

<span data-ttu-id="16094-116">このチュートリアルでは、ASP.NET Web API で CORS のサポートについて説明します。</span><span class="sxs-lookup"><span data-stu-id="16094-116">This tutorial demonstrates CORS support in ASP.NET Web API.</span></span> <span data-ttu-id="16094-117">まず、– 1 つと呼ばれる"WebService"、Web API コント ローラーをホストして、その他の呼び出された"WebClient"、web サービスを呼び出すの 2 つの ASP.NET プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="16094-117">We'll start by creating two ASP.NET projects – one called "WebService", which hosts a Web API controller, and the other called "WebClient", which calls WebService.</span></span> <span data-ttu-id="16094-118">別のドメインでは、2 つのアプリケーションがホストされている、ため WebClient から web サービスへの AJAX 要求は、クロス オリジン要求です。</span><span class="sxs-lookup"><span data-stu-id="16094-118">Because the two applications are hosted at different domains, an AJAX request from WebClient to WebService is a cross-origin request.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image1.png)

### <a name="what-is-same-origin"></a><span data-ttu-id="16094-119">「同じ発生元」とは何ですか。</span><span class="sxs-lookup"><span data-stu-id="16094-119">What is "Same Origin"?</span></span>

<span data-ttu-id="16094-120">2 つの Url では、同じスキーム、ホスト、およびポートがある場合の原点が同じがあります。</span><span class="sxs-lookup"><span data-stu-id="16094-120">Two URLs have the same origin if they have identical schemes, hosts, and ports.</span></span> <span data-ttu-id="16094-121">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span><span class="sxs-lookup"><span data-stu-id="16094-121">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span></span>

<span data-ttu-id="16094-122">これら 2 つの Url は、原点が同じであります。</span><span class="sxs-lookup"><span data-stu-id="16094-122">These two URLs have the same origin:</span></span>

- `http://example.com/foo.html`
- `http://example.com/bar.html`

<span data-ttu-id="16094-123">これらの Url は、2 つよりも前の別の原点をあります。</span><span class="sxs-lookup"><span data-stu-id="16094-123">These URLs have different origins than the previous two:</span></span>

- <span data-ttu-id="16094-124">`http://example.net`-別のドメイン</span><span class="sxs-lookup"><span data-stu-id="16094-124">`http://example.net` - Different domain</span></span>
- <span data-ttu-id="16094-125">`http://example.com:9000/foo.html`-別のポート</span><span class="sxs-lookup"><span data-stu-id="16094-125">`http://example.com:9000/foo.html` - Different port</span></span>
- <span data-ttu-id="16094-126">`https://example.com/foo.html`-別のスキーム</span><span class="sxs-lookup"><span data-stu-id="16094-126">`https://example.com/foo.html` - Different scheme</span></span>
- <span data-ttu-id="16094-127">`http://www.example.com/foo.html`-別のサブドメイン</span><span class="sxs-lookup"><span data-stu-id="16094-127">`http://www.example.com/foo.html` - Different subdomain</span></span>

> [!NOTE]
> <span data-ttu-id="16094-128">Internet Explorer では、元のドメインを比較するときに、ポートは考慮されません。</span><span class="sxs-lookup"><span data-stu-id="16094-128">Internet Explorer does not consider the port when comparing origins.</span></span>


<a id="create-webapi-project"></a>
## <a name="create-the-webservice-project"></a><span data-ttu-id="16094-129">Web サービス プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="16094-129">Create the WebService Project</span></span>

> [!NOTE]
> <span data-ttu-id="16094-130">このセクションでは、Web API プロジェクトを作成する方法を既に知っているものとします。</span><span class="sxs-lookup"><span data-stu-id="16094-130">This section assumes you already know how to create Web API projects.</span></span> <span data-ttu-id="16094-131">いない場合を参照してください。 [ASP.NET Web API の使用を開始する](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)です。</span><span class="sxs-lookup"><span data-stu-id="16094-131">If not, see [Getting Started with ASP.NET Web API](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>


<span data-ttu-id="16094-132">Visual Studio を起動し、新しい作成**ASP.NET Web アプリケーション**プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="16094-132">Start Visual Studio and create a new **ASP.NET Web Application** project.</span></span> <span data-ttu-id="16094-133">選択、**空**プロジェクト テンプレート。</span><span class="sxs-lookup"><span data-stu-id="16094-133">Select the **Empty** project template.</span></span> <span data-ttu-id="16094-134">「フォルダーの追加し、コアの参照」を選択、 **Web API**チェック ボックスをオンします。</span><span class="sxs-lookup"><span data-stu-id="16094-134">Under "Add folders and core references for", select the **Web API** checkbox.</span></span> <span data-ttu-id="16094-135">必要に応じて、アプリを Azure にデプロイ Mircosoft「クラウドのホスト」オプションを選択します。</span><span class="sxs-lookup"><span data-stu-id="16094-135">Optionally, select the "Host in Cloud" option to deploy the app to Mircosoft Azure.</span></span> <span data-ttu-id="16094-136">マイクロソフトでは、無料の web ホスティングで最大 10 個の web サイトを提供しています、[無料試用版の Azure アカウント](https://azure.microsoft.com/free/?WT.mc_id=A443DD604)です。</span><span class="sxs-lookup"><span data-stu-id="16094-136">Microsoft offers free web hosting for up to 10 websites in a [free Azure trial account](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span></span>

[![](enabling-cross-origin-requests-in-web-api/_static/image3.png)](enabling-cross-origin-requests-in-web-api/_static/image2.png)

<span data-ttu-id="16094-137">という名前の Web API コント ローラーを追加`TestController`を次のコード。</span><span class="sxs-lookup"><span data-stu-id="16094-137">Add a Web API controller named `TestController` with the following code:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample1.cs)]

<span data-ttu-id="16094-138">アプリケーションをローカルで実行したり、Azure にデプロイできます。</span><span class="sxs-lookup"><span data-stu-id="16094-138">You can run the application locally or deploy to Azure.</span></span> <span data-ttu-id="16094-139">(このチュートリアルでは、スクリーン ショットを展開した Azure App Service Web Apps にします。)Web API が動作していることを確認するに移動`http://hostname/api/test/`ここで、 *hostname*アプリケーションの展開先のドメインです。</span><span class="sxs-lookup"><span data-stu-id="16094-139">(For the screenshots in this tutorial, I deployed to Azure App Service Web Apps.) To verify that the web API is working, navigate to `http://hostname/api/test/`, where *hostname* is the domain where you deployed the application.</span></span> <span data-ttu-id="16094-140">応答テキストが表示されます&quot;を取得します。 テスト メッセージ&quot;です。</span><span class="sxs-lookup"><span data-stu-id="16094-140">You should see the response text, &quot;GET: Test Message&quot;.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image4.png)

<a id="create-client"></a>
## <a name="create-the-webclient-project"></a><span data-ttu-id="16094-141">WebClient プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="16094-141">Create the WebClient Project</span></span>

<span data-ttu-id="16094-142">ASP.NET Web アプリケーションの別のプロジェクトを作成し、選択、 **MVC**プロジェクト テンプレート。</span><span class="sxs-lookup"><span data-stu-id="16094-142">Create another ASP.NET Web Application project and select the **MVC** project template.</span></span> <span data-ttu-id="16094-143">必要に応じて、選択**認証の変更** > **認証なし**です。</span><span class="sxs-lookup"><span data-stu-id="16094-143">Optionally, select **Change Authentication** > **No Authentication**.</span></span> <span data-ttu-id="16094-144">このチュートリアルでは、認証が不要です。</span><span class="sxs-lookup"><span data-stu-id="16094-144">You don't need authentication for this tutorial.</span></span>

[![](enabling-cross-origin-requests-in-web-api/_static/image6.png)](enabling-cross-origin-requests-in-web-api/_static/image5.png)

<span data-ttu-id="16094-145">ソリューション エクスプ ローラーで、Views/Home/Index.cshtml ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="16094-145">In Solution Explorer, open the file Views/Home/Index.cshtml.</span></span> <span data-ttu-id="16094-146">次のようにこのファイル内のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="16094-146">Replace the code in this file with the following:</span></span>

[!code-cshtml[Main](enabling-cross-origin-requests-in-web-api/samples/sample2.cshtml?highlight=13)]

<span data-ttu-id="16094-147">*ServiceUrl*変数、web サービス アプリケーションの URI を使用します。</span><span class="sxs-lookup"><span data-stu-id="16094-147">For the *serviceUrl* variable, use the URI of the WebService app.</span></span> <span data-ttu-id="16094-148">今すぐ WebClient アプリをローカルで実行または別の web サイトに発行します。</span><span class="sxs-lookup"><span data-stu-id="16094-148">Now run the WebClient app locally or publish it to another website.</span></span>

<span data-ttu-id="16094-149">表示されている HTTP メソッドを使用して、web サービス アプリケーションに AJAX 要求を送信するボタンをクリックして"再試行"(GET、POST または PUT) ボックスの一覧です。</span><span class="sxs-lookup"><span data-stu-id="16094-149">Clicking the "Try It" button submits an AJAX request to the WebService app, using the HTTP method listed in the dropdown box (GET, POST, or PUT).</span></span> <span data-ttu-id="16094-150">これにより、別のクロス オリジン要求をチェックします。</span><span class="sxs-lookup"><span data-stu-id="16094-150">This lets us examine different cross-origin requests.</span></span> <span data-ttu-id="16094-151">現在、web サービス アプリケーションは、CORS をサポートしていませんエラーが発生する場合は、ボタンをクリックするようにします。</span><span class="sxs-lookup"><span data-stu-id="16094-151">Right now, the WebService app does not support CORS, so if you click the button, you will get an error.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image7.png)

> [!NOTE]
> <span data-ttu-id="16094-152">ツールで、HTTP トラフィックを監視する場合と同様に[Fiddler](http://www.telerik.com/fiddler)ブラウザーは、GET 要求を送信し、要求が成功するしますが、AJAX 呼び出しがエラーを返しますが表示されます。</span><span class="sxs-lookup"><span data-stu-id="16094-152">If you watch the HTTP traffic in a tool like [Fiddler](http://www.telerik.com/fiddler), you will see that the browser does send the GET request, and the request succeeds, but the AJAX call returns an error.</span></span> <span data-ttu-id="16094-153">同一生成元ポリシーがからブラウザーを禁止していないことを理解することが重要*送信*要求します。</span><span class="sxs-lookup"><span data-stu-id="16094-153">It's important to understand that same-origin policy does not prevent the browser from *sending* the request.</span></span> <span data-ttu-id="16094-154">代わりに、アプリケーションが表示されるを防止、*応答*です。</span><span class="sxs-lookup"><span data-stu-id="16094-154">Instead, it prevents the application from seeing the *response*.</span></span>


![](enabling-cross-origin-requests-in-web-api/_static/image8.png)

<a id="enable-cors"></a>
## <a name="enable-cors"></a><span data-ttu-id="16094-155">CORS を有効にします。</span><span class="sxs-lookup"><span data-stu-id="16094-155">Enable CORS</span></span>

<span data-ttu-id="16094-156">今すぐ WebService アプリで CORS が有効にしてみましょう。</span><span class="sxs-lookup"><span data-stu-id="16094-156">Now let's enable CORS in the WebService app.</span></span> <span data-ttu-id="16094-157">最初に、CORS の NuGet パッケージを追加します。</span><span class="sxs-lookup"><span data-stu-id="16094-157">First, add the CORS NuGet package.</span></span> <span data-ttu-id="16094-158">Visual Studio から、**ツール**メニューの **ライブラリ パッケージ マネージャー**選択してから、 **Package Manager Console**です。</span><span class="sxs-lookup"><span data-stu-id="16094-158">In Visual Studio, from the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="16094-159">パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="16094-159">In the Package Manager Console window, type the following command:</span></span>

[!code-powershell[Main](enabling-cross-origin-requests-in-web-api/samples/sample3.ps1)]

<span data-ttu-id="16094-160">このコマンドは、最新のパッケージをインストールし、コア Web API ライブラリを含むすべての依存関係を更新します。</span><span class="sxs-lookup"><span data-stu-id="16094-160">This command installs the latest package and updates all dependencies, including the core Web API libraries.</span></span> <span data-ttu-id="16094-161">ユーザーを特定のバージョンを対象とするバージョン フラグ。</span><span class="sxs-lookup"><span data-stu-id="16094-161">User the -Version flag to target a specific version.</span></span> <span data-ttu-id="16094-162">CORS パッケージには、Web API 2.0 以降が必要です。</span><span class="sxs-lookup"><span data-stu-id="16094-162">The CORS package requires Web API 2.0 or later.</span></span>

<span data-ttu-id="16094-163">アプリのファイルを開く\_Start/WebApiConfig.cs です。</span><span class="sxs-lookup"><span data-stu-id="16094-163">Open the file App\_Start/WebApiConfig.cs.</span></span> <span data-ttu-id="16094-164">次のコードを追加、 **WebApiConfig.Register**メソッドです。</span><span class="sxs-lookup"><span data-stu-id="16094-164">Add the following code to the **WebApiConfig.Register** method.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample4.cs?highlight=9)]

<span data-ttu-id="16094-165">次に、追加、 **[EnableCors]**属性を`TestController`クラス。</span><span class="sxs-lookup"><span data-stu-id="16094-165">Next, add the **[EnableCors]** attribute to the `TestController` class:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample5.cs?highlight=3,7)]

<span data-ttu-id="16094-166">*オリジン*パラメーター、WebClient アプリケーションを配置した URI を使用します。</span><span class="sxs-lookup"><span data-stu-id="16094-166">For the *origins* parameter, use the URI where you deployed the WebClient application.</span></span> <span data-ttu-id="16094-167">これにより、クロス オリジン要求から WebClient、まだ他のすべてのクロス ドメイン要求を許可中にします。</span><span class="sxs-lookup"><span data-stu-id="16094-167">This allows cross-origin requests from WebClient, while still disallowing all other cross-domain requests.</span></span> <span data-ttu-id="16094-168">パラメーターを後で説明します**[EnableCors]**で詳しく説明します。</span><span class="sxs-lookup"><span data-stu-id="16094-168">Later, I'll describe the parameters for **[EnableCors]** in more detail.</span></span>

<span data-ttu-id="16094-169">末尾にスラッシュを含めないでください、*オリジン*URL。</span><span class="sxs-lookup"><span data-stu-id="16094-169">Do not include a forward slash at the end of the *origins* URL.</span></span>

<span data-ttu-id="16094-170">更新された web サービス アプリケーションを再展開します。</span><span class="sxs-lookup"><span data-stu-id="16094-170">Redeploy the updated WebService application.</span></span> <span data-ttu-id="16094-171">WebClient を更新する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="16094-171">You don't need to update WebClient.</span></span> <span data-ttu-id="16094-172">WebClient から AJAX 要求が成功するようになりました。</span><span class="sxs-lookup"><span data-stu-id="16094-172">Now the AJAX request from WebClient should succeed.</span></span> <span data-ttu-id="16094-173">GET、PUT、POST メソッドはすべて許可します。</span><span class="sxs-lookup"><span data-stu-id="16094-173">The GET, PUT, and POST methods are all allowed.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image9.png)

<a id="how-it-works"></a>
## <a name="how-cors-works"></a><span data-ttu-id="16094-174">CORS のしくみ</span><span class="sxs-lookup"><span data-stu-id="16094-174">How CORS Works</span></span>

<span data-ttu-id="16094-175">このセクションでは、HTTP メッセージのレベルでの CORS 要求での動作について説明します。</span><span class="sxs-lookup"><span data-stu-id="16094-175">This section describes what happens in a CORS request, at the level of the HTTP messages.</span></span> <span data-ttu-id="16094-176">構成できるようにするために、CORS のしくみを理解することが重要、 **[EnableCors]**正しく、属性し、期待どおりに機能しない場合のトラブルシューティングを行います。</span><span class="sxs-lookup"><span data-stu-id="16094-176">It's important to understand how CORS works, so that you can configure the **[EnableCors]** attribute correctly, and troubleshoot if things don't work as you expect.</span></span>

<span data-ttu-id="16094-177">CORS の仕様には、クロス オリジン要求を有効にするいくつかの新しい HTTP ヘッダーが導入されています。</span><span class="sxs-lookup"><span data-stu-id="16094-177">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="16094-178">ブラウザーでは、CORS をサポートする場合、クロス オリジン要求を自動的にこれらのヘッダーを設定します。JavaScript コードで特別な何もする必要はありません。</span><span class="sxs-lookup"><span data-stu-id="16094-178">If a browser supports CORS, it sets these headers automatically for cross-origin requests; you don't need to do anything special in your JavaScript code.</span></span>

<span data-ttu-id="16094-179">クロス オリジン要求の例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="16094-179">Here is an example of a cross-origin request.</span></span> <span data-ttu-id="16094-180">"Origin"ヘッダーは、要求を行っているサイトのドメインを示します。</span><span class="sxs-lookup"><span data-stu-id="16094-180">The "Origin" header gives the domain of the site that is making the request.</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample6.cmd?highlight=5)]

<span data-ttu-id="16094-181">サーバーは、要求を許可している場合は、アクセス コントロール-を許可する-オリジン ヘッダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="16094-181">If the server allows the request, it sets the Access-Control-Allow-Origin header.</span></span> <span data-ttu-id="16094-182">このヘッダーの値は、Origin ヘッダーと一致するか、ワイルドカード文字は、"\*"、すべてのオリジンを許可されていることを意味します。</span><span class="sxs-lookup"><span data-stu-id="16094-182">The value of this header either matches the Origin header, or is the wildcard value "\*", meaning that any origin is allowed.</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample7.cmd?highlight=5)]

<span data-ttu-id="16094-183">応答に、アクセス コントロール-を許可する-オリジン ヘッダーが含まれていない場合、AJAX 要求は失敗します。</span><span class="sxs-lookup"><span data-stu-id="16094-183">If the response does not include the Access-Control-Allow-Origin header, the AJAX request fails.</span></span> <span data-ttu-id="16094-184">具体的には、ブラウザーには、要求が許可されていません。</span><span class="sxs-lookup"><span data-stu-id="16094-184">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="16094-185">サーバーでは、正常な応答を返す、場合でも、ブラウザーは行いません応答クライアント アプリケーションで使用できます。</span><span class="sxs-lookup"><span data-stu-id="16094-185">Even if the server returns a successful response, the browser does not make the response available to the client application.</span></span>

<span data-ttu-id="16094-186">**プレフライト要求**</span><span class="sxs-lookup"><span data-stu-id="16094-186">**Preflight Requests**</span></span>

<span data-ttu-id="16094-187">いくつかの CORS 要求については、ブラウザーは、リソースの実際の要求を送信する前に「プレフライト要求を」と呼ばれる、追加の要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="16094-187">For some CORS requests, the browser sends an additional request, called a "preflight request", before it sends the actual request for the resource.</span></span>

<span data-ttu-id="16094-188">ブラウザーは、次の条件に該当する場合、プレフライト要求を省略できます。</span><span class="sxs-lookup"><span data-stu-id="16094-188">The browser can skip the preflight request if the following conditions are true:</span></span>

- <span data-ttu-id="16094-189">要求メソッドが GET、HEAD、または POST、*と*</span><span class="sxs-lookup"><span data-stu-id="16094-189">The request method is GET, HEAD, or POST, *and*</span></span>
- <span data-ttu-id="16094-190">アプリケーションは Accept、Accept-language、Content-language 以外の任意の要求ヘッダーを設定していないコンテンツの種類、または最後のイベント ID、*と*</span><span class="sxs-lookup"><span data-stu-id="16094-190">The application does not set any request headers other than Accept, Accept-Language, Content-Language, Content-Type, or Last-Event-ID, *and*</span></span>
- <span data-ttu-id="16094-191">Content-type ヘッダー (場合に設定) は、次のいずれか。</span><span class="sxs-lookup"><span data-stu-id="16094-191">The Content-Type header (if set) is one of the following:</span></span> 

    - <span data-ttu-id="16094-192">application/x-www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="16094-192">application/x-www-form-urlencoded</span></span>
    - <span data-ttu-id="16094-193">マルチパート フォーム データ</span><span class="sxs-lookup"><span data-stu-id="16094-193">multipart/form-data</span></span>
    - <span data-ttu-id="16094-194">テキスト/プレーン</span><span class="sxs-lookup"><span data-stu-id="16094-194">text/plain</span></span>

<span data-ttu-id="16094-195">アプリケーションで呼び出すことによって設定されたヘッダーに要求ヘッダーについて、規則が適用されます**setRequestHeader**上、 **XMLHttpRequest**オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="16094-195">The rule about request headers applies to headers that the application sets by calling **setRequestHeader** on the **XMLHttpRequest** object.</span></span> <span data-ttu-id="16094-196">(CORS の仕様は、これら「作成者要求ヘッダー」を呼び出します)。このルールは、ヘッダーには適用されません、*ブラウザー*ユーザー エージェント、ホスト、またはコンテンツの長さなど、設定できます。</span><span class="sxs-lookup"><span data-stu-id="16094-196">(The CORS specification calls these "author request headers".) The rule does not apply to headers the *browser* can set, such as User-Agent, Host, or Content-Length.</span></span>

<span data-ttu-id="16094-197">プレフライト要求の例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="16094-197">Here is an example of a preflight request:</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample8.cmd?highlight=4-5)]

<span data-ttu-id="16094-198">事前要求は、HTTP OPTIONS メソッドを使用します。</span><span class="sxs-lookup"><span data-stu-id="16094-198">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="16094-199">2 つの特殊なヘッダーが含まれています。</span><span class="sxs-lookup"><span data-stu-id="16094-199">It includes two special headers:</span></span>

- <span data-ttu-id="16094-200">アクセス コントロール-要求メソッド: 実際の要求に使用される HTTP メソッド。</span><span class="sxs-lookup"><span data-stu-id="16094-200">Access-Control-Request-Method: The HTTP method that will be used for the actual request.</span></span>
- <span data-ttu-id="16094-201">アクセス コントロール-要求ヘッダー。 要求ヘッダーの一覧を、*アプリケーション*実際の要求に設定します。</span><span class="sxs-lookup"><span data-stu-id="16094-201">Access-Control-Request-Headers: A list of request headers that the *application* set on the actual request.</span></span> <span data-ttu-id="16094-202">(ここでも、これは含まれません、ブラウザーを設定するヘッダー。)</span><span class="sxs-lookup"><span data-stu-id="16094-202">(Again, this does not include headers that the browser sets.)</span></span>

<span data-ttu-id="16094-203">次に、応答の例、サーバーで要求を許可すると仮定した場合を示します。</span><span class="sxs-lookup"><span data-stu-id="16094-203">Here is an example response, assuming that the server allows the request:</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample9.cmd?highlight=6-7)]

<span data-ttu-id="16094-204">応答には、許可されているメソッドを一覧表示するアクセスの制御の許可する-メソッド ヘッダーおよび必要に応じて、アクセス コントロール-を許可する-ヘッダー ヘッダー、許可されているヘッダーの一覧が含まれています。</span><span class="sxs-lookup"><span data-stu-id="16094-204">The response includes an Access-Control-Allow-Methods header that lists the allowed methods, and optionally an Access-Control-Allow-Headers header, which lists the allowed headers.</span></span> <span data-ttu-id="16094-205">プレフライト要求が成功した場合、ブラウザーは、前述のとおり、実際の要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="16094-205">If the preflight request succeeds, the browser sends the actual request, as described earlier.</span></span>

<a id="scope"></a>
## <a name="scope-rules-for-enablecors"></a><span data-ttu-id="16094-206">[EnableCors] の規則のスコープ</span><span class="sxs-lookup"><span data-stu-id="16094-206">Scope Rules for [EnableCors]</span></span>

<span data-ttu-id="16094-207">アプリケーションでは、アクション、コント ローラーごとまたはグローバルのすべての Web API コント ローラーあたり CORS を有効にできます。</span><span class="sxs-lookup"><span data-stu-id="16094-207">You can enable CORS per action, per controller, or globally for all Web API controllers in your application.</span></span>

<span data-ttu-id="16094-208">**アクションごと**</span><span class="sxs-lookup"><span data-stu-id="16094-208">**Per Action**</span></span>

<span data-ttu-id="16094-209">1 つのアクションで CORS を有効にする設定、 **[EnableCors]**アクション メソッドの属性です。</span><span class="sxs-lookup"><span data-stu-id="16094-209">To enable CORS for a single action, set the **[EnableCors]** attribute on the action method.</span></span> <span data-ttu-id="16094-210">次の例の CORS の有効、`GetItem`メソッドのみです。</span><span class="sxs-lookup"><span data-stu-id="16094-210">The following example enables CORS for the `GetItem` method only.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample10.cs)]

<span data-ttu-id="16094-211">**コント ローラーあたり**</span><span class="sxs-lookup"><span data-stu-id="16094-211">**Per Controller**</span></span>

<span data-ttu-id="16094-212">設定した場合**[EnableCors]**コント ローラー クラスに、コント ローラーのすべてのアクションに適用します。</span><span class="sxs-lookup"><span data-stu-id="16094-212">If you set **[EnableCors]** on the controller class, it applies to all the actions on the controller.</span></span> <span data-ttu-id="16094-213">アクションの CORS を無効にする追加の**[DisableCors]**属性をアクションにします。</span><span class="sxs-lookup"><span data-stu-id="16094-213">To disable CORS for an action, add the **[DisableCors]** attribute to the action.</span></span> <span data-ttu-id="16094-214">次の例では、CORS を有効を除くすべてのメソッドに対する`PutItem`です。</span><span class="sxs-lookup"><span data-stu-id="16094-214">The following example enables CORS for every method except `PutItem`.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample11.cs)]

<span data-ttu-id="16094-215">**グローバル**</span><span class="sxs-lookup"><span data-stu-id="16094-215">**Globally**</span></span>

<span data-ttu-id="16094-216">アプリケーション内のすべての Web API コント ローラーで CORS を有効にするのには、渡す、 **EnableCorsAttribute**インスタンスを**EnableCors**メソッド。</span><span class="sxs-lookup"><span data-stu-id="16094-216">To enable CORS for all Web API controllers in your application, pass an **EnableCorsAttribute** instance to the **EnableCors** method:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample12.cs)]

<span data-ttu-id="16094-217">優先順位の順序は、1 つ以上のスコープ内で属性を設定する場合です。</span><span class="sxs-lookup"><span data-stu-id="16094-217">If you set the attribute at more than one scope, the order of precedence is:</span></span>

1. <span data-ttu-id="16094-218">操作</span><span class="sxs-lookup"><span data-stu-id="16094-218">Action</span></span>
2. <span data-ttu-id="16094-219">コントローラー</span><span class="sxs-lookup"><span data-stu-id="16094-219">Controller</span></span>
3. <span data-ttu-id="16094-220">Global</span><span class="sxs-lookup"><span data-stu-id="16094-220">Global</span></span>

<a id="allowed-origins"></a>
## <a name="set-the-allowed-origins"></a><span data-ttu-id="16094-221">許可されるオリジンを設定します。</span><span class="sxs-lookup"><span data-stu-id="16094-221">Set the Allowed Origins</span></span>

<span data-ttu-id="16094-222">*オリジン*のパラメーター、 **[EnableCors]**属性は、どのオリジンはリソースへのアクセス許可を指定します。</span><span class="sxs-lookup"><span data-stu-id="16094-222">The *origins* parameter of the **[EnableCors]** attribute specifies which origins are allowed to access the resource.</span></span> <span data-ttu-id="16094-223">値は、許可されるオリジンのコンマ区切りの一覧です。</span><span class="sxs-lookup"><span data-stu-id="16094-223">The value is a comma-separated list of the allowed origins.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample13.cs)]

<span data-ttu-id="16094-224">ワイルドカード文字を使用することもできます。"\*"すべてのオリジンから要求を許可します。</span><span class="sxs-lookup"><span data-stu-id="16094-224">You can also use the wildcard value "\*" to allow requests from any origins.</span></span>

<span data-ttu-id="16094-225">すべてのオリジンからの要求を許可する前に慎重に検討してください。</span><span class="sxs-lookup"><span data-stu-id="16094-225">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="16094-226">これは、事実上あらゆる web サイトが、web API への AJAX 呼び出しを実行できることを意味します。</span><span class="sxs-lookup"><span data-stu-id="16094-226">It means that literally any website can make AJAX calls to your web API.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample14.cs)]

<a id="allowed-methods"></a>
## <a name="set-the-allowed-http-methods"></a><span data-ttu-id="16094-227">許可される HTTP メソッドを設定します。</span><span class="sxs-lookup"><span data-stu-id="16094-227">Set the Allowed HTTP Methods</span></span>

<span data-ttu-id="16094-228">*メソッド*のパラメーター、 **[EnableCors]**属性は、どの HTTP メソッドは、リソースへのアクセス許可を指定します。</span><span class="sxs-lookup"><span data-stu-id="16094-228">The *methods* parameter of the **[EnableCors]** attribute specifies which HTTP methods are allowed to access the resource.</span></span> <span data-ttu-id="16094-229">すべてのメソッドを許可するワイルドカード値を使用"\*"です。</span><span class="sxs-lookup"><span data-stu-id="16094-229">To allow all methods, use the wildcard value "\*".</span></span> <span data-ttu-id="16094-230">次の例では、GET および POST 要求だけを許可します。</span><span class="sxs-lookup"><span data-stu-id="16094-230">The following example allows only GET and POST requests.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample15.cs)]

<a id="allowed-request-headers"></a>
## <a name="set-the-allowed-request-headers"></a><span data-ttu-id="16094-231">許可されている要求ヘッダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="16094-231">Set the Allowed Request Headers</span></span>

<span data-ttu-id="16094-232">既に説明しましたプレフライト要求が、アクセス コントロール-要求ヘッダー ヘッダーが含まれて、アプリケーションによって設定される HTTP ヘッダーの一覧を表示する (いわゆる"要求ヘッダーを author") です。</span><span class="sxs-lookup"><span data-stu-id="16094-232">Earlier I described how a preflight request might include an Access-Control-Request-Headers header, listing the HTTP headers set by the application (the so-called "author request headers").</span></span> <span data-ttu-id="16094-233">*ヘッダー*のパラメーター、 **[EnableCors]**属性を指定する作成者要求ヘッダーが許可されます。</span><span class="sxs-lookup"><span data-stu-id="16094-233">The *headers* parameter of the **[EnableCors]** attribute specifies which author request headers are allowed.</span></span> <span data-ttu-id="16094-234">すべてのヘッダーは、次のように設定します。*ヘッダー*に"\*"です。</span><span class="sxs-lookup"><span data-stu-id="16094-234">To allow any headers, set *headers* to "\*".</span></span> <span data-ttu-id="16094-235">ホワイト リストの特定のヘッダーを次のように設定します。*ヘッダー*許可されたヘッダーのコンマ区切りの一覧にします。</span><span class="sxs-lookup"><span data-stu-id="16094-235">To whitelist specific headers, set *headers* to a comma-separated list of the allowed headers:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample16.cs)]

<span data-ttu-id="16094-236">ただし、ブラウザーでは、アクセス コントロール-要求ヘッダーを設定する方法の完全一貫性がありません。</span><span class="sxs-lookup"><span data-stu-id="16094-236">However, browsers are not entirely consistent in how they set Access-Control-Request-Headers.</span></span> <span data-ttu-id="16094-237">たとえば、Chrome 現在含まれています"origin"です。FireFox では、スクリプトでアプリケーションを設定する場合でも、"Accept"などの標準ヘッダーは含まれません中に。</span><span class="sxs-lookup"><span data-stu-id="16094-237">For example, Chrome currently includes "origin"; while FireFox does not include standard headers such as "Accept", even when the application sets them in script.</span></span>

<span data-ttu-id="16094-238">設定した場合*ヘッダー*以外の値を"\*"、する必要がありますを含めるには、少なくとも「受け入れる」、「コンテンツの種類」と「発生元」、およびサポートする任意のカスタム ヘッダー。</span><span class="sxs-lookup"><span data-stu-id="16094-238">If you set *headers* to anything other than "\*", you should include at least "accept", "content-type", and "origin", plus any custom headers that you want to support.</span></span>

<a id="allowed-response-headers"></a>
## <a name="set-the-allowed-response-headers"></a><span data-ttu-id="16094-239">許可されている応答ヘッダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="16094-239">Set the Allowed Response Headers</span></span>

<span data-ttu-id="16094-240">既定では、ブラウザーは公開しませんすべてのアプリケーションに応答ヘッダー。</span><span class="sxs-lookup"><span data-stu-id="16094-240">By default, the browser does not expose all of the response headers to the application.</span></span> <span data-ttu-id="16094-241">既定で利用できる応答ヘッダーは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="16094-241">The response headers that are available by default are:</span></span>

- <span data-ttu-id="16094-242">キャッシュ制御</span><span class="sxs-lookup"><span data-stu-id="16094-242">Cache-Control</span></span>
- <span data-ttu-id="16094-243">コンテンツの言語</span><span class="sxs-lookup"><span data-stu-id="16094-243">Content-Language</span></span>
- <span data-ttu-id="16094-244">コンテンツの種類</span><span class="sxs-lookup"><span data-stu-id="16094-244">Content-Type</span></span>
- <span data-ttu-id="16094-245">有効期限が切れる</span><span class="sxs-lookup"><span data-stu-id="16094-245">Expires</span></span>
- <span data-ttu-id="16094-246">最終更新</span><span class="sxs-lookup"><span data-stu-id="16094-246">Last-Modified</span></span>
- <span data-ttu-id="16094-247">プラグマ</span><span class="sxs-lookup"><span data-stu-id="16094-247">Pragma</span></span>

<span data-ttu-id="16094-248">CORS の仕様を呼び出す[単純な応答ヘッダー](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header)です。</span><span class="sxs-lookup"><span data-stu-id="16094-248">The CORS spec calls these [simple response headers](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header).</span></span> <span data-ttu-id="16094-249">その他のヘッダーをアプリケーションで使用できるようにする設定、 *exposedHeaders*のパラメーター **[EnableCors]**です。</span><span class="sxs-lookup"><span data-stu-id="16094-249">To make other headers available to the application, set the *exposedHeaders* parameter of **[EnableCors]**.</span></span>

<span data-ttu-id="16094-250">次の例で、コント ローラーの`Get`メソッドが 'X カスタム ヘッダー' という名前のカスタム ヘッダーを設定します。</span><span class="sxs-lookup"><span data-stu-id="16094-250">In the following example, the controller's `Get` method sets a custom header named ‘X-Custom-Header'.</span></span> <span data-ttu-id="16094-251">既定では、ブラウザーでクロス オリジン要求では、このヘッダーは公開されません。</span><span class="sxs-lookup"><span data-stu-id="16094-251">By default, the browser will not expose this header in a cross-origin request.</span></span> <span data-ttu-id="16094-252">ヘッダーを使用できるようにするに 'X カスタム ヘッダー' を含める*exposedHeaders*です。</span><span class="sxs-lookup"><span data-stu-id="16094-252">To make the header available, include ‘X-Custom-Header' in *exposedHeaders*.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample17.cs)]

<a id="credentials"></a>
## <a name="passing-credentials-in-cross-origin-requests"></a><span data-ttu-id="16094-253">クロス オリジン要求に資格情報を渡す</span><span class="sxs-lookup"><span data-stu-id="16094-253">Passing Credentials in Cross-Origin Requests</span></span>

<span data-ttu-id="16094-254">資格情報では、CORS 要求で特別な処理が必要です。</span><span class="sxs-lookup"><span data-stu-id="16094-254">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="16094-255">既定では、ブラウザーは、クロス オリジン要求に資格情報を送信しません。</span><span class="sxs-lookup"><span data-stu-id="16094-255">By default, the browser does not send any credentials with a cross-origin request.</span></span> <span data-ttu-id="16094-256">Cookie と、HTTP 認証スキームの資格情報が含まれます。</span><span class="sxs-lookup"><span data-stu-id="16094-256">Credentials include cookies as well as HTTP authentication schemes.</span></span> <span data-ttu-id="16094-257">クロス オリジン要求に資格情報を送信するクライアントを設定する必要があります**XMLHttpRequest.withCredentials** true に設定します。</span><span class="sxs-lookup"><span data-stu-id="16094-257">To send credentials with a cross-origin request, the client must set **XMLHttpRequest.withCredentials** to true.</span></span>

<span data-ttu-id="16094-258">使用して**XMLHttpRequest**直接。</span><span class="sxs-lookup"><span data-stu-id="16094-258">Using **XMLHttpRequest** directly:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample18.cs)]

<span data-ttu-id="16094-259">JQuery: で</span><span class="sxs-lookup"><span data-stu-id="16094-259">In jQuery:</span></span>

[!code-javascript[Main](enabling-cross-origin-requests-in-web-api/samples/sample19.js)]

<span data-ttu-id="16094-260">さらに、サーバーは、資格情報を許可する必要があります。</span><span class="sxs-lookup"><span data-stu-id="16094-260">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="16094-261">Web API のクロス オリジンの資格情報を許可する設定、 **SupportsCredentials**プロパティは true を**[EnableCors]**属性。</span><span class="sxs-lookup"><span data-stu-id="16094-261">To allow cross-origin credentials in Web API, set the **SupportsCredentials** property to true on the **[EnableCors]** attribute:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample20.cs)]

<span data-ttu-id="16094-262">このプロパティが true の場合、HTTP 応答ヘッダーには、アクセス コントロール-を許可する-資格情報が含まれます。</span><span class="sxs-lookup"><span data-stu-id="16094-262">If this property is true, the HTTP response will include an Access-Control-Allow-Credentials header.</span></span> <span data-ttu-id="16094-263">このヘッダーは、クロス オリジン要求の資格情報を許可することをブラウザーに指示します。</span><span class="sxs-lookup"><span data-stu-id="16094-263">This header tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="16094-264">ブラウザーが資格情報を送信しますが、応答は有効なアクセス制御を許可する-資格情報ヘッダーを含まない、ブラウザーは、アプリケーションへの応答を公開しないと、AJAX 要求が失敗します。</span><span class="sxs-lookup"><span data-stu-id="16094-264">If the browser sends credentials, but the response does not include a valid Access-Control-Allow-Credentials header, the browser will not expose the response to the application, and the AJAX request fails.</span></span>

<span data-ttu-id="16094-265">設定するには十分に注意**SupportsCredentials**を true にため、別のドメインで web サイトはユーザーに気付かれることがなく、ユーザーの代理で Web API のログイン ユーザーの資格情報を送信することができます。</span><span class="sxs-lookup"><span data-stu-id="16094-265">Be very careful about setting **SupportsCredentials** to true, because it means a website at another domain can send a logged-in user's credentials to your Web API on the user's behalf, without the user being aware.</span></span> <span data-ttu-id="16094-266">CORS の仕様もその設定を示す*オリジン*に&quot; \* &quot;が正しくない場合**SupportsCredentials**が true です。</span><span class="sxs-lookup"><span data-stu-id="16094-266">The CORS spec also states that setting *origins* to &quot;\*&quot; is invalid if **SupportsCredentials** is true.</span></span>

<a id="cors-policy-providers"></a>
## <a name="custom-cors-policy-providers"></a><span data-ttu-id="16094-267">カスタムの CORS ポリシー プロバイダー</span><span class="sxs-lookup"><span data-stu-id="16094-267">Custom CORS Policy Providers</span></span>

<span data-ttu-id="16094-268">**[EnableCors]**属性を実装して、 **ICorsPolicyProvider**インターフェイスです。</span><span class="sxs-lookup"><span data-stu-id="16094-268">The **[EnableCors]** attribute implements the **ICorsPolicyProvider** interface.</span></span> <span data-ttu-id="16094-269">派生するクラスを作成して、独自の実装を指定できます**属性**を実装して**ICorsProlicyProvider**です。</span><span class="sxs-lookup"><span data-stu-id="16094-269">You can provide your own implementation by creating a class that derives from **Attribute** and implements **ICorsProlicyProvider**.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample21.cs)]

<span data-ttu-id="16094-270">これで、任意の場所にする場合は、属性を適用する**[EnableCors]**です。</span><span class="sxs-lookup"><span data-stu-id="16094-270">Now you can apply the attribute any place that you would put **[EnableCors]**.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample22.cs)]

<span data-ttu-id="16094-271">たとえば、カスタム CORS ポリシー プロバイダーは、構成ファイルから設定を読み取る可能性があります。</span><span class="sxs-lookup"><span data-stu-id="16094-271">For example, a custom CORS policy provider could read the settings from a configuration file.</span></span>

<span data-ttu-id="16094-272">属性を使用する代わりに、として登録することができます、 **ICorsPolicyProviderFactory**オブジェクトを作成する**ICorsPolicyProvider**オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="16094-272">As an alternative to using attributes, you can register an **ICorsPolicyProviderFactory** object that creates **ICorsPolicyProvider** objects.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample23.cs)]

<span data-ttu-id="16094-273">設定する、 **ICorsPolicyProviderFactory**を呼び出し、 **SetCorsPolicyProviderFactory**起動時に次のように、拡張メソッド。</span><span class="sxs-lookup"><span data-stu-id="16094-273">To set the **ICorsPolicyProviderFactory**, call the **SetCorsPolicyProviderFactory** extension method at startup, as follows:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample24.cs)]

<a id="browser-support"></a>
## <a name="browser-support"></a><span data-ttu-id="16094-274">ブラウザー サポート</span><span class="sxs-lookup"><span data-stu-id="16094-274">Browser Support</span></span>

<span data-ttu-id="16094-275">Web API CORS パッケージは、サーバー側テクノロジです。</span><span class="sxs-lookup"><span data-stu-id="16094-275">The Web API CORS package is a server-side technology.</span></span> <span data-ttu-id="16094-276">ユーザーのブラウザーは、CORS をサポートするためにも必要です。</span><span class="sxs-lookup"><span data-stu-id="16094-276">The user's browser also needs to support CORS.</span></span> <span data-ttu-id="16094-277">幸いにも、すべての主要なブラウザーの現在のバージョンが含まれて[CORS のサポート](http://caniuse.com/cors)です。</span><span class="sxs-lookup"><span data-stu-id="16094-277">Fortunately, the current versions of all major browsers include [support for CORS](http://caniuse.com/cors).</span></span>

<span data-ttu-id="16094-278">Internet Explorer 8 および Internet Explorer 9 XMLHttpRequest ではなく、レガシ XDomainRequest オブジェクトを使用して、CORS の部分的なサポートに必要があります。</span><span class="sxs-lookup"><span data-stu-id="16094-278">Internet Explorer 8 and Internet Explorer 9 have partial support for CORS, using the legacy XDomainRequest object instead of XMLHttpRequest.</span></span> <span data-ttu-id="16094-279">詳細については、次を参照してください。 [XDomainRequest の制限事項、制限事項と回避策](https://blogs.msdn.com/b/ieinternals/archive/2010/05/13/xdomainrequest-restrictions-limitations-and-workarounds.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="16094-279">For more information, see [XDomainRequest - Restrictions, Limitations and Workarounds](https://blogs.msdn.com/b/ieinternals/archive/2010/05/13/xdomainrequest-restrictions-limitations-and-workarounds.aspx).</span></span>