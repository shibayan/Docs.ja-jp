---
title: "ASP.NET Core MVC と Visual Studio の概要"
author: rick-anderson
description: "ASP.NET Core MVC と Visual Studio の概要"
keywords: ASP.NET Core, MVC
ms.author: riande
manager: wpickett
ms.date: 10/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 72852678a296107e6a2a146ed2324335a6e870ee
ms.sourcegitcommit: 77b8025c30ec2fd46d85ee2a2b497c44435d3009
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/12/2018
---
# <a name="getting-started-with-aspnet-core-mvc-and-visual-studio"></a><span data-ttu-id="30403-104">ASP.NET Core MVC と Visual Studio の概要</span><span class="sxs-lookup"><span data-stu-id="30403-104">Getting started with ASP.NET Core MVC and Visual Studio</span></span>

<span data-ttu-id="30403-105">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="30403-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[consider RP](../../includes/razor.md)]

<span data-ttu-id="30403-106">このチュートリアルには 3 つのバージョンがあります。</span><span class="sxs-lookup"><span data-stu-id="30403-106">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="30403-107">macOS: [Visual Studio for Mac を使用して ASP.NET Core MVC アプリを作成する](xref:tutorials/first-mvc-app-mac/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="30403-107">macOS: [Create an ASP.NET Core MVC app with Visual Studio for Mac](xref:tutorials/first-mvc-app-mac/start-mvc)</span></span>
* <span data-ttu-id="30403-108">Windows: [Visual Studio を使用して ASP.NET Core MVC アプリを作成する](xref:tutorials/first-mvc-app/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="30403-108">Windows: [Create an ASP.NET Core MVC app with Visual Studio](xref:tutorials/first-mvc-app/start-mvc)</span></span>
* <span data-ttu-id="30403-109">macOS、Linux、Windows: [Visual Studio Code を使用して ASP.NET Core MVC アプリを作成する](xref:tutorials/first-mvc-app-xplat/start-mvc)</span><span class="sxs-lookup"><span data-stu-id="30403-109">macOS, Linux, and Windows: [Create an ASP.NET Core MVC app with Visual Studio Code](xref:tutorials/first-mvc-app-xplat/start-mvc)</span></span>

## <a name="install-visual-studio-and-net-core"></a><span data-ttu-id="30403-110">Visual Studio と .NET Core のインストール</span><span class="sxs-lookup"><span data-stu-id="30403-110">Install Visual Studio and .NET Core</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="30403-111">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="30403-111">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="30403-112">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="30403-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="30403-113">Visual Studio Community 2017 をインストールします。</span><span class="sxs-lookup"><span data-stu-id="30403-113">Install Visual Studio Community 2017.</span></span> <span data-ttu-id="30403-114">コミュニティ ダウンロードを選択します。</span><span class="sxs-lookup"><span data-stu-id="30403-114">Select the Community download.</span></span> <span data-ttu-id="30403-115">Visual Studio 2017 をインストールしている場合は、この手順をスキップします。</span><span class="sxs-lookup"><span data-stu-id="30403-115">Skip this step if you have Visual Studio 2017 installed.</span></span>

* [<span data-ttu-id="30403-116">Visual Studio 2017 ホーム ページのインストーラー</span><span class="sxs-lookup"><span data-stu-id="30403-116">Visual Studio 2017 Home page installer</span></span>](https://www.visualstudio.com/)

<span data-ttu-id="30403-117">インストーラーを実行し、次のワークロードを選択します。</span><span class="sxs-lookup"><span data-stu-id="30403-117">Run the installer and select the following workloads:</span></span>

* <span data-ttu-id="30403-118">**ASP.NET と Web 開発** (**[Web & Cloud]\(Web とクラウド\)** の下)</span><span class="sxs-lookup"><span data-stu-id="30403-118">**ASP.NET and web development** (under **Web & Cloud**)</span></span>
* <span data-ttu-id="30403-119">**.NET Core クロスプラットフォームの開発** (**[他のツールセット]** の下)</span><span class="sxs-lookup"><span data-stu-id="30403-119">**.NET Core cross-platform development** (under **Other Toolsets**)</span></span>

![**ASP.NET と Web の開発ツール** (**[Web & Cloud]\(Web とクラウド\)** の下)](start-mvc/_static/web_workload.png)

![**.NET Core クロスクロスプラットフォームの開発** (**[他のツールセット]** の下)](start-mvc/_static/x_plat_wl.png)

---

## <a name="create-a-web-app"></a><span data-ttu-id="30403-122">Web アプリの作成</span><span class="sxs-lookup"><span data-stu-id="30403-122">Create a web app</span></span>

<span data-ttu-id="30403-123">Visual Studio で **[ファイル]、[新規作成]、[プロジェクト]** の順に選択します。</span><span class="sxs-lookup"><span data-stu-id="30403-123">From Visual Studio, select  **File > New > Project**.</span></span>

![[ファイル] > [新規] > [プロジェクト]](start-mvc/_static/alt_new_project.png)

<span data-ttu-id="30403-125">**[新しいプロジェクト]** ダイアログで次のように設定します。</span><span class="sxs-lookup"><span data-stu-id="30403-125">Complete the **New Project** dialog:</span></span>

* <span data-ttu-id="30403-126">左側のウィンドウで、**[.NET Core]** をタップします。</span><span class="sxs-lookup"><span data-stu-id="30403-126">In the left pane, tap **.NET Core**</span></span>
* <span data-ttu-id="30403-127">中央のウィンドウで、**[ASP.NET Core Web Application (.NET Core)]** をタップします。</span><span class="sxs-lookup"><span data-stu-id="30403-127">In the center pane, tap **ASP.NET Core Web Application (.NET Core)**</span></span>
* <span data-ttu-id="30403-128">プロジェクトに "MvcMovie" と名前を付けます (コードをコピーするときに名前空間が一致するようにするために、プロジェクトに "MvcMovie" と名前を付けることが重要です)。</span><span class="sxs-lookup"><span data-stu-id="30403-128">Name the project "MvcMovie" (It's important to name the project "MvcMovie" so when you copy code, the namespace will match.)</span></span>
* <span data-ttu-id="30403-129">**[OK]** をタップします</span><span class="sxs-lookup"><span data-stu-id="30403-129">Tap **OK**</span></span>

![<span data-ttu-id="30403-130">[新しいプロジェクト] ダイアログ、左ウィンドウの .NET Core、ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="30403-130">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project2.png)


# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="30403-131">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="30403-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="30403-132">**[ASP.NET Core Web Application (.NET Core) - MvcMovie]** ダイアログを次のように設定します。</span><span class="sxs-lookup"><span data-stu-id="30403-132">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="30403-133">バージョン セレクター ドロップダウン ボックスで、**[ASP.NET Core 2.-]** を選択します</span><span class="sxs-lookup"><span data-stu-id="30403-133">In the version selector drop-down box select **ASP.NET Core 2.-**</span></span>
* <span data-ttu-id="30403-134">**[Web Application(Model-View-Controller)]** を選択します</span><span class="sxs-lookup"><span data-stu-id="30403-134">Select **Web Application(Model-View-Controller)**</span></span>
* <span data-ttu-id="30403-135">**[OK]** をタップします。</span><span class="sxs-lookup"><span data-stu-id="30403-135">Tap **OK**.</span></span>

![<span data-ttu-id="30403-136">[新しいプロジェクト] ダイアログ、左ウィンドウの .NET Core、ASP.NET Core Web</span><span class="sxs-lookup"><span data-stu-id="30403-136">New project dialog, .Net core in left pane, ASP.NET Core web</span></span> ](start-mvc/_static/new_project22.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="30403-137">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="30403-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="30403-138">**[ASP.NET Core Web Application (.NET Core) - MvcMovie]** ダイアログを次のように設定します。</span><span class="sxs-lookup"><span data-stu-id="30403-138">Complete the **New ASP.NET Core Web Application (.NET Core) - MvcMovie** dialog:</span></span>

* <span data-ttu-id="30403-139">バージョン セレクター ドロップダウン ボックスで、**[ASP.NET Core 1.1]** をタップします</span><span class="sxs-lookup"><span data-stu-id="30403-139">In the version selector drop-down box tap **ASP.NET Core 1.1**</span></span>
* <span data-ttu-id="30403-140">**[Web アプリケーション]** をタップします</span><span class="sxs-lookup"><span data-stu-id="30403-140">Tap **Web Application**</span></span>
* <span data-ttu-id="30403-141">既定の **[No Authentication]\(認証なし\)** のままにします</span><span class="sxs-lookup"><span data-stu-id="30403-141">Keep the default **No Authentication**</span></span>
* <span data-ttu-id="30403-142">**[OK]** をタップします。</span><span class="sxs-lookup"><span data-stu-id="30403-142">Tap **OK**.</span></span>

![新しい ASP.NET Core Web アプリ](start-mvc/_static/p3.png)

---

<span data-ttu-id="30403-144">Visual Studio は、作成した MVC プロジェクトの既定のテンプレートを使用しました。</span><span class="sxs-lookup"><span data-stu-id="30403-144">Visual Studio used a default template for the MVC project you just created.</span></span> <span data-ttu-id="30403-145">プロジェクト名を入力し、いくつかのオプションを選択すると、すぐに作業アプリができあがります。</span><span class="sxs-lookup"><span data-stu-id="30403-145">You have a working app right now by entering a project name and selecting a few options.</span></span> <span data-ttu-id="30403-146">これは単純なスターター プロジェクトなので、ここから始めることをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="30403-146">This is a simple starter project, and it's a good place to start,</span></span>

<span data-ttu-id="30403-147">**F5** キーをタップしてデバッグ モードでアプリを実行します。非デバッグ モードで実行する場合は **Ctrl + F5** キーを押します。</span><span class="sxs-lookup"><span data-stu-id="30403-147">Tap **F5** to run the app in debug mode or **Ctrl-F5** in non-debug mode.</span></span>
<!-- These images are also used by uid: tutorials/first-mvc-app-xplat/start-mvc -->
![実行中のアプリ](start-mvc/_static/1.png)

* <span data-ttu-id="30403-149">Visual Studio で [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) が開始され、アプリが実行されます。</span><span class="sxs-lookup"><span data-stu-id="30403-149">Visual Studio starts [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="30403-150">アドレス バーには、`example.com` などではなく、`localhost:port#` が表示されます。</span><span class="sxs-lookup"><span data-stu-id="30403-150">Notice that the address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="30403-151">これは、`localhost` がローカル コンピューターの標準のホスト名であるためです。</span><span class="sxs-lookup"><span data-stu-id="30403-151">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="30403-152">Visual Studio が Web プロジェクトを作成する場合は、Web サーバーにランダム ポートが使用されます。</span><span class="sxs-lookup"><span data-stu-id="30403-152">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="30403-153">上の図で、ポート番号は 5000 です。</span><span class="sxs-lookup"><span data-stu-id="30403-153">In the image above, the port number is 5000.</span></span> <span data-ttu-id="30403-154">ブラウザーの URL は `localhost:5000` を示します。</span><span class="sxs-lookup"><span data-stu-id="30403-154">The URL in the browser shows `localhost:5000`.</span></span> <span data-ttu-id="30403-155">アプリを実行する際には、別のポート番号が表示されます。</span><span class="sxs-lookup"><span data-stu-id="30403-155">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="30403-156">**Ctrl + F5** キー (非デバッグ モード) でアプリを起動することで、コードの変更、ファイルの保存、ブラウザーの更新、およびコード変更の確認を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="30403-156">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="30403-157">多くの開発者は、すばやくアプリを起動し、変更を確認できる非デバッグ モードの使用を好みます。</span><span class="sxs-lookup"><span data-stu-id="30403-157">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>
* <span data-ttu-id="30403-158">**[デバッグ]** メニュー項目から、デバッグ モードまたは非デバッグ モードでアプリを起動できます。</span><span class="sxs-lookup"><span data-stu-id="30403-158">You can launch the app in debug or non-debug mode from the **Debug** menu item:</span></span>

![[デバッグ] メニュー](start-mvc/_static/debug_menu.png)

* <span data-ttu-id="30403-160">**[IIS Express]** ボタンをタップしてアプリをデバッグできます</span><span class="sxs-lookup"><span data-stu-id="30403-160">You can debug the app by tapping the **IIS Express** button</span></span>

![IIS Express](start-mvc/_static/iis_express.png)

<span data-ttu-id="30403-162">既定のテンプレートには、作業用の **[Home]、[About]**、**[Contact]** のリンクがあります。</span><span class="sxs-lookup"><span data-stu-id="30403-162">The default template gives you working **Home, About** and **Contact** links.</span></span> <span data-ttu-id="30403-163">上のブラウザーの画像には、これらのリンクが表示されていません。</span><span class="sxs-lookup"><span data-stu-id="30403-163">The browser image above doesn't show these links.</span></span> <span data-ttu-id="30403-164">ブラウザーのサイズによっては、ナビゲーション アイコンをクリックしてリンクを表示する必要がある場合があります。</span><span class="sxs-lookup"><span data-stu-id="30403-164">Depending on the size of your browser, you might need to click the navigation icon to show them.</span></span>

![右上のナビゲーション アイコン](start-mvc/_static/2.png)

<span data-ttu-id="30403-166">デバッグ モードで実行した場合は、**Shift + F5** キーをタップしてデバッグを停止します。</span><span class="sxs-lookup"><span data-stu-id="30403-166">If you were running in debug mode, tap **Shift-F5** to stop debugging.</span></span>

<span data-ttu-id="30403-167">このチュートリアルの次のパートでは、MVC について説明し、コードの作成を開始します。</span><span class="sxs-lookup"><span data-stu-id="30403-167">In the next part of this tutorial, we'll learn about MVC and start writing some code.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="30403-168">次へ</span><span class="sxs-lookup"><span data-stu-id="30403-168">Next</span></span>](adding-controller.md)  