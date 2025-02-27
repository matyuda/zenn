---
title: "Reactの準備・作成"
free: true
---
# このチャプターでやること

Figmaで生成されたコードをReactに取り込み、Amplifyにデプロイを行います。

# amplify pull
Cloud9に戻り、コンソールに対して、前チャプターのamplify pullコマンドを実施
以下のようなコマンドです。
``` sh
cd ~/environment/amplify-homes/
amplify pull --appId d2st1248z19swk --envName homesoda
```

以下コマンドを実施すると、UI Libraryで生成したレイアウトがコードとして取り込めていることが分かります。
``` sh
ls -l /home/ec2-user/environment/amplify-homes/src/ui-components/NewHomes.jsx
cat /home/ec2-user/environment/amplify-homes/src/ui-components/NewHomes.jsx
```

# src/App.jsの修正

/home/ec2-user/environment/amplify-homes/src/App.jsを以下コードで差し替えます。
```
import './App.css';
import { NewHomes, NavBar, MarketingFooter } from './ui-components'

function App() {
  return (
    <div className="App">
    <NavBar />
    <NewHomes />
    <MarketingFooter />
    </div>
  );
}

export default App;
```

# 関連するファイルの修正

後述の amplify publish した際に発生する mergeVariantsAndOverrides に関するエラーメッセージを回避するため、２ファイルについて、以下のとおり変更を行い、コメントアウトします。

/home/ec2-user/environment/amplify-homes/src/ui-components/HeroLayout1.jsx
```
12c12
<   mergeVariantsAndOverrides,
---
> //  mergeVariantsAndOverrides,
52a53
>   /*
56a58
>   */
68c70
<       {...getOverrideProps(overrides, "HeroLayout1")}
---
> //      {...getOverrideProps(overrides, "HeroLayout1")}
```

/home/ec2-user/environment/amplify-homes/src/ui-components/MyIcon.jsx
```
12c12
<   mergeVariantsAndOverrides,
---
> //  mergeVariantsAndOverrides,
304a305
>   /*
308a310
>   */
326c328
<       {...getOverrideProps(overrides, "MyIcon")}
---
> //      {...getOverrideProps(overrides, "MyIcon")}
```

# amplify publish
作成された結果をpublishしましょう。`[DEP0148] DeprecationWarning`というWorningが出ますが、お気にせず。
```sh
amplify publish
```

以下のような表示がされればOKです。ブラウザでアクセスしてみましょう
```
✔ Zipping artifacts completed.
✔ Deployment complete!
https://homesoda.d2st1248z19swk.amplifyapp.com
```

# 確認結果
![](https://storage.googleapis.com/zenn-user-upload/28a5d28e5dec-20220227.png)


# src/App.jsの修正2

/home/ec2-user/environment/amplify-homes/src/App.jsを以下コードで差し替えます。
これによりページングや幅変更でコンテンツが調整されるようになります。
```
import './App.css';
import { NewHomes, NavBar, MarketingFooter } from './ui-components'

function App() {
  return (
    <div className="App">
    <NavBar width={"100vw"}/>
    <NewHomes isPaginated itemsPerPage={3}/>
    <MarketingFooter width={"100vw"}/>
    </div>
  );
}

export default App;
```


# amplify publish２
作成された結果をpublishしましょう。`[DEP0148] DeprecationWarning`というWorningが出ますが、お気にせず。
```sh
amplify publish
```

以下のような表示がされればOKです。ブラウザでアクセスしてみましょう
```
✔ Zipping artifacts completed.
✔ Deployment complete!
https://homesoda.d2st1248z19swk.amplifyapp.com
```

# 確認結果２
![](https://storage.googleapis.com/zenn-user-upload/67c121bbc71f-20220227.png)