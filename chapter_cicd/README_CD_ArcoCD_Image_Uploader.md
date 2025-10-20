このドキュメントは、CD(Countinuous Delivery)のハンズオン資料になります。  
ハンズオンのChapter_argocdにある、ArcoCDを利用して実施していきます。  
そのため、ArgoCDをインストールしていない場合には、以下サイトからインストールを実施してください。  
ArgoCDの詳細については、[こちら](https://github.com/cloudnativedaysjp/cnd-handson/blob/main/chapter_argocd/README_webui.md)を参照ください。  

また、このドキュメントについては、CICDのハンズオンにCD部分の拡張版として、<B>ArcoCD Image Updater</B> を使うことを  
目的としたものになります。  
ArgoCD Image Updaterの参照URL (https://argocd-image-updater.readthedocs.io/en/stable/)

# ArgoCD Image Updater とは

ArgoCDについては、ArgoCDの章にて説明がされていますが、今回利用する ArgoCD Image Updater は、Kubernetes の  
ワークロードでデプロイされているコンテナイメージの新しいバージョンを検出し、ArgoCD を使って許可された最新バージョンへ  
自動的に更新させることが可能で、ArgoCDで動作しているアプリケーションに対して適切なアプリケーションパラメータを設定する  
ことで動作します。使い方は簡単で、更新対象となるイメージの一覧と書くイメージのバージョンをArgoCDのアプリケーションリソースに  
Annotationとして付与します。その後、ArgoCD Image Updaterは、ArgoCDから設定済みのアプリケーションを定期的にポーリングし、  
対応するコンテナレジストリに新しい    バージョンがないかを確認し、レジストリに新しいバージョンが見つかり、バージョン制約を  
満たしていれば、ArgoCDに対して新しいイメージでアプリケーションを更新します。 
ArgoCDとの密接な統合により、Sync Windows や Application リソースに対する RBAC 認可などの高度な機能も完全にサポートされています。  

## ArgoCD Image Updaterが動作する条件
・　ArgoCDが動作していること
・　マニフェストが、KustomizeやHelmで管理させれていること
・　コンテナイメージリポジトリに使う認証情報は、ArgoCD Image updaterと同じクラスター上で存在すること
・　ArgoCD Image Updaterでは、Rollback機能がないため、ArgoCD側で対応、Roadmap上にはあるがまだ未定。


# ArgoCD Image Updater で管理したいアプリケーションをArgoCDにデプロイ
```
kubectl apply -f ./manifest/application_argocdupdate.yaml
```




# ArgoCD Image Updater のインストール
### 以下のリンクがあるが、今回namespaceなど異なるため、内容を変更しています。
https://argocd-image-updater.readthedocs.io/en/stable/install/installation/

変更したものについては、chapter_cicd/app/にあります。
```
kubectl apply -f ./manifest/argocd_image_uploader.yaml
```


