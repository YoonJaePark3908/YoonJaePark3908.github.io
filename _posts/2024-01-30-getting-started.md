---
title: 깃허브 블로그 개설 완료
author: Yoon Jaepark
date: 2024-01-30 14:49:00 +0900
categories: [Android, Tech]
tags: [getting started]
pin: false
img_path: '/posts/20180809'
---

## 티스토리에서 깃허브로 이전한 이유

### 깔끔한 테마와 코드블럭

티스토리 블로그의 테마와 코드블럭은 개인적으로 아쉬운 마음이 많이 들었습니다. 특히나 코드블럭은 아쉬운 마음이 많아서 여러가지 테마를 알아봤지만 결국 마음에 드는
테마를 찾을 수 없었습니다. 직접 테마를 커스터마이징 하는 방법도 있지만, 그러기에는 들어가는 리소스가 많다고 판단하여 보류하면서 블로그를 이용해 왔습니다.
그러다가 여러 개발 관련 지식이 담긴 글을 읽어오면서 깃허브 블로그가 있다는것을 알게 되었고, 깃허브 블로그의 대부분의 UI가 매우 깔끔하며 코드 블럭 또한 저의
마음을 사로 잡았습니다. 깃허브 블로그는 jekyll 프레임워크를 사용하여 커스터마이징 테마가 상당히 많습니다. 그 중 가장 마음에 들었던 테마는 [**Chirpy**](https://github.com/cotes2020/jekyll-theme-chirpy) 이였고 바로 적용하였습니다.
- 티스토리 블로그 코드블럭
<img width="822" alt="tistory_code_block" src="https://github.com/YoonJaePark3908/StockPortfolio/assets/54883589/c06ab3b8-ac76-4846-8370-41c3aa51fae2">
- 깃허브 블로그 코드블럭
``` kotlin
fun pathToBitmap(path: String?): Bitmap? {
    return try {
        val f = File(path)
        val options = BitmapFactory.Options()
        options.inPreferredConfig = Bitmap.Config.ARGB_8888
        BitmapFactory.decodeStream(FileInputStream(f), null, options)
    } catch (e: Exception) {
        e.printStackTrace()
        null
    }
}
```

###  

Sign in to GitHub and browse to [**Chirpy Starter**][starter], click the button <kbd>Use this template</kbd> > <kbd>Create a new repository</kbd>, and name the new repository `USERNAME.github.io`, where `USERNAME` represents your GitHub username.

### 

Sign in to GitHub to [fork **Chirpy**](https://github.com/cotes2020/jekyll-theme-chirpy/fork), and then rename it to `USERNAME.github.io` (`USERNAME` means your username).

Next, clone your site to local machine. In order to build JavaScript files later, we need to install [Node.js][nodejs], and then run the tool:

```console
$ bash tools/init
```

> If you don't want to deploy your site on GitHub Pages, append option `--no-gh` at the end of the above command.
{: .prompt-info }

The above command will:

1. Check out the code to the [latest tag][latest-tag] (to ensure the stability of your site: as the code for the default branch is under development).
2. Remove non-essential sample files and take care of GitHub-related files.
3. Build JavaScript files and export to `assets/js/dist/`{: .filepath }, then make them tracked by Git.
4. Automatically create a new commit to save the changes above.

### Installing Dependencies

Before running local server for the first time, go to the root directory of your site and run:

```console
$ bundle
```

## Usage

### Configuration

Update the variables of `_config.yml`{: .filepath} as needed. Some of them are typical options:

- `url`
- `avatar`
- `timezone`
- `lang`

### Social Contact Options

Social contact options are displayed at the bottom of the sidebar. You can turn on/off the specified contacts in file `_data/contact.yml`{: .filepath }.

### Customizing Stylesheet

If you need to customize the stylesheet, copy the theme's `assets/css/jekyll-theme-chirpy.scss`{: .filepath} to the same path on your Jekyll site, and then add the custom style at the end of it.

Starting with version `6.2.0`, if you want to overwrite the SASS variables defined in `_sass/addon/variables.scss`{: .filepath}, copy the main sass file `_sass/main.scss`{: .filepath} into the `_sass`{: .filepath} directory in your site's source, then create a new file `_sass/variables-hook.scss`{: .filepath} and assign new value.

### Customing Static Assets

Static assets configuration was introduced in version `5.1.0`. The CDN of the static assets is defined by file `_data/origin/cors.yml`{: .filepath }, and you can replace some of them according to the network conditions in the region where your website is published.

Also, if you'd like to self-host the static assets, please refer to the [_chirpy-static-assets_](https://github.com/cotes2020/chirpy-static-assets#readme).

### Running Local Server

You may want to preview the site contents before publishing, so just run it by:

```kotlin
fun test() {
  val name = "윤재박"
}
```

After a few seconds, the local service will be published at _<http://127.0.0.1:4000>_.

## Deployment

Before the deployment begins, check out the file `_config.yml`{: .filepath} and make sure the `url` is configured correctly. Furthermore, if you prefer the [**project site**](https://help.github.com/en/github/working-with-github-pages/about-github-pages#types-of-github-pages-sites) and don't use a custom domain, or you want to visit your website with a base URL on a web server other than **GitHub Pages**, remember to change the `baseurl` to your project name that starts with a slash, e.g, `/project-name`.

### Deploy by Using GitHub Actions

There are a few things to get ready for.

- If you're on the GitHub Free plan, keep your site repository public.
- If you have committed `Gemfile.lock`{: .filepath} to the repository, and your local machine is not running Linux, go to the root of your site and update the platform list of the lock-file:

  ```kotlin
  val name = "윤재박"
  ```

1. Browse to your repository on GitHub. Select the tab _Settings_, then click _Pages_ in the left navigation bar. Then, in the **Source** section (under _Build and deployment_), select [**GitHub Actions**][pages-workflow-src] from the dropdown menu.  

2. Push any commits to GitHub to trigger the _Actions_ workflow. In the _Actions_ tab of your repository, you should see the workflow _Build and Deploy_ running. Once the build is complete and successful, the site will be deployed automatically.

At this point, you can go to the URL indicated by GitHub to access your site.

### Manually Build and Deploy

On self-hosted servers, you cannot enjoy the convenience of **GitHub Actions**. Therefore, you should build the site on your local machine and then upload the site files to the server.

Go to the root of the source project, and build your site as follows:

```console
$ JEKYLL_ENV=production bundle exec jekyll b
```

Unless you specified the output path, the generated site files will be placed in folder `_site`{: .filepath} of the project's root directory. Now you should upload those files to the target server.

[nodejs]: https://nodejs.org/
[starter]: https://github.com/cotes2020/chirpy-starter
[pages-workflow-src]: https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site#publishing-with-a-custom-github-actions-workflow
[latest-tag]: https://github.com/cotes2020/jekyll-theme-chirpy/tags
