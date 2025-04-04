---
layout: blog
title: "SIG Nodeの紹介"
slug: sig-node-spotlight-2024
canonicalUrl: https://www.kubernetes.dev/blog/2024/06/20/sig-node-spotlight-2024
date: 2024-06-20
author: >
  Arpit Agrawal
translator: >
  Taisuke Okamoto (IDCフロンティア),
  [Junya Okabe](https://github.com/Okabe-Junya) (筑波大学)
---

コンテナオーケストレーションの世界で、[Kubernetes](/ja)は圧倒的な存在感を示しており、世界中で最も複雑で動的なアプリケーションの一部を動かしています。
その裏では、Special Interest Groups(SIG)のネットワークがKubernetesの革新と安定性を牽引しています。

今日は、SIG Nodeのメンバーである[Matthias Bertschy](https://www.linkedin.com/in/matthias-bertschy-b427b815/)、[Gunju Kim](https://www.linkedin.com/in/gunju-kim-916b33190/)、[Sergey Kanzhelev](https://www.linkedin.com/in/sergeykanzhelev/)にお話を伺い、彼らの役割、課題、そして[SIG Node](https://github.com/kubernetes/community/blob/master/sig-node/README.md)内の注目すべき取り組みについて光を当てていきます。

_複数の回答者による共同回答の場合は、回答者全員のイニシャルで表記します。_

## 自己紹介

**Arpit:** 本日はお時間をいただき、ありがとうございます。まず、自己紹介とSIG Node内での役割について簡単に教えていただけますか？

**Matthias:** Matthias Bertschyと申します。フランス人で、フランスアルプスの近く、ジュネーブ湖のそばに住んでいます。2017年からKubernetesのコントリビューターとして活動し、SIG Nodeのレビュアー、そして[Prow](https://docs.prow.k8s.io/docs/overview/)のメンテナーを務めています。現在は、[ARMO](https://www.armosec.io/)というセキュリティスタートアップでシニアKubernetes開発者として働いています。ARMOは、[Kubescape](https://www.cncf.io/projects/kubescape/)というプロジェクトをCNCFに寄贈しました。

![ジュネーブ湖とアルプス](Lake_Geneva_and_the_Alps.jpg)

**Gunju:** Gunju Kimと申します。[NAVER](https://www.navercorp.com/naver/naverMain)でソフトウェアエンジニアとして働いており、検索サービス用のクラウドプラットフォームの開発に注力しています。2021年から空き時間を使ってKubernetesプロジェクトにコントリビュートしています。

**Sergey:** Sergey Kanzhelevと申します。3年間Kubernetesと[Google Kubernetes Engine](https://cloud.google.com/kubernetes-engine)に携わり、長年オープンソースプロジェクトに取り組んできました。現在はSIG Nodeの議長を務めています。

## SIG Nodeについて

**Arpit:** ありがとうございます！Kubernetesエコシステム内でのSIG Nodeの責任について、読者の方々に概要を説明していただけますか？

**M/G/S:** SIG NodeはKubernetesで最初に、あるいは最初期に設立されたSIGの1つです。このSIGは、KubernetesとNodeリソースとのすべてのやり取り、そしてNode自体のメンテナンスに責任を持っています。これはかなり広範囲に及び、SIGはKubernetesのコードベースの大部分を所有しています。この広範な所有権のため、SIG NodeはSIG Network、SIG Storage、SIG Securityなど他のSIGと常に連絡を取り合っており、Kubernetesの新機能や開発のほとんどが何らかの形でSIG Nodeに関わっています。

**Arpit**: SIG NodeはKubernetesのパフォーマンスと安定性にどのように貢献していますか？

**M/G/S:** Kubernetesは、安価なハードウェアを搭載した小型の物理VMから、大規模なAI/ML最適化されたGPU搭載Nodeまで、さまざまなサイズと形状のNodeで動作します。Nodeは数か月オンラインのままの場合もあれば、クラウドプロバイダーの余剰コンピューティングで実行されているため、短命で任意のタイミングでプリエンプトされる可能性もあります。

Node上のKubernetesエージェントである[`kubelet`](/ja/docs/concepts/overview/components/#kubelet)は、これらすべての環境で確実に動作する必要があります。
近年、`kubelet`の操作パフォーマンスの重要性が増しています。
その理由は二つあります。
一つは、Kubernetesが通信や小売業などの分野で、より小規模なNodeで使用されるようになってきており、可能な限り小さなリソース消費(フットプリント)で動作することが求められているからです。
もう一つは、AI/MLワークロードでは各Nodeが非常に高価なため、操作の遅延がわずか1秒でも計算コストに大きな影響を与える可能性があるからです。

## 課題と機会

**Arpit:** SIG Nodeが今後直面すると予想される課題や可能性について、どのようなものがあるでしょうか？

**M/G/S:** Kubernetesが誕生から10年を迎え、次の10年に向かう中で、新しい種類のワークロードへの対応が強く求められています。SIG Nodeはこの取り組みで重要な役割を果たすことになるでしょう。後ほど詳しく説明しますが、サイドカーのKEPは、こうした新しいタイプのワークロードをサポートするための取り組みの一例です。

今後数年間の主な課題は、既存の機能の品質と後方互換性を維持しつつ、いかに革新を続けていくかということです。
SIG Nodeは、これからもKubernetesの開発において中心的な役割を担い続けるでしょう。

**Arpit:** SIG Nodeで現在取り組んでいる研究や開発分野の中で、特に注目しているものはありますか？

**M/G/S:** 新しいタイプのワークロードへの対応は、私たちにとって非常に興味深い分野です。最近取り組んでいるサイドカーコンテナの研究はその好例といえるでしょう。サイドカーは、アプリケーションの中核となるコードを変更することなく、その機能を拡張できる柔軟なソリューションを提供します。

**Arpit:** SIG Nodeを維持する上で直面した課題と、それをどのように克服したかを教えてください。

**M/G/S:** SIG Nodeが直面する最大の課題は、その広範な責任範囲と数多くの機能要望への対応です。この課題に取り組むため、私たちは新たなレビュアーの参加を積極的に呼びかけています。また、常にプロセスの改善に努め、フィードバックに迅速に対応できる体制を整えています。さらに、各リリースの後にはSIG Nodeのミーティングでフィードバックセッションを開催し、問題点や改善が必要な分野を特定し、具体的な行動計画を立てています。

**Arpit:** SIG Nodeが現在注目している技術や、Kubernetesへの導入を検討している新しい機能などはありますか？

**M/G/S:** SIG Nodeは、Kubernetesが依存しているさまざまなコンポーネントの開発に積極的に関与し、その進展を注意深く見守っています。これには、[コンテナランタイム]((/ja/docs/setup/production-environment/container-runtimes/))([containerd](https://containerd.io/)や[CRI-O](https://cri-o.io/)など)やOSの機能が含まれます。例えば、現在 _cgroup v1_ の廃止と削除が迫っていますが、これに対してKubernetesユーザーが円滑に移行できるよう、SIG NodeとKubernetesプロジェクト全体で取り組んでいます。また、containerdがバージョン`2.0`をリリースする予定ですが、これには非推奨機能の削除が含まれており、Kubernetesユーザーにも影響が及ぶと考えられます。

**Arpit:** SIG Nodeのメンテナーとしての経験の中で、特に誇りに思う思い出深い経験や成果を共有していただけますか？

**Mathias:** 最高の瞬間は、私の最初のKEP([`startupProbe`](/ja/docs/concepts/workloads/pods/pod-lifecycle/#container-probes)の導入)がついにGA(General Availability)に昇格したときだと思います。また、私の貢献がコントリビューターによって日々使用されているのを見るのも楽しいです。例えば、スカッシュコミットにもかかわらずLGTMを保持するために使用されるGitHubツリーハッシュを含むコメントなどです。

## サイドカーコンテナ

**Arpit:** Kubernetesの文脈におけるサイドカーコンテナの概念とその進化について、もう少し詳しく教えていただけますか？

**M/G/S:** [サイドカーコンテナ](/ja/docs/concepts/workloads/pods/sidecar-containers/)の概念は、Kubernetesが複合コンテナのアイデアを導入した2015年にさかのぼります。同じPod内でメインのアプリケーションコンテナと並行して実行されるこれらの追加コンテナは、コアのコードベースを変更することなくアプリケーションの機能を拡張・強化する方法として見られていました。サイドカーの初期の採用者はカスタムスクリプトと設定を使用して管理していましたが、このアプローチは一貫性とスケーラビリティの面で課題がありました。

**Arpit:** サイドカーコンテナが特に有益な具体的なユースケースや例を共有していただけますか？

**M/G/S:** サイドカーコンテナは、さまざまな方法でアプリケーションの機能を強化するために使用できる多用途なツールです:

- **ロギングとモニタリング:** サイドカーコンテナを使用して、Pod内の主要アプリケーションコンテナからログとメトリクスを収集し、中央のロギングおよびモニタリングシステムに送信できます。
- **トラフィックのフィルタリングとルーティング:** サイドカーコンテナを使用して、Pod内の主要アプリケーションコンテナとの間のトラフィックをフィルタリングおよびルーティングできます。
- **暗号化と復号化:** サイドカーコンテナを使用して、Pod内の主要アプリケーションコンテナと外部サービスの間で流れるデータを暗号化および復号化できます。
- **データ同期:** サイドカーコンテナを使用して、Pod内の主要アプリケーションコンテナと外部データベースやサービスの間でデータを同期できます。
- **フォールトインジェクション:** サイドカーコンテナを使用して、Pod内の主要アプリケーションコンテナに障害を注入し、障害に対する耐性をテストできます。

**Arpit:** 提案によると、一部の企業がサイドカー機能を追加したKubernetesのフォークを使用しているそうです。この機能の採用状況やコミュニティの関心度について、何か見解をお聞かせいただけますか？

**M/G/S:** 採用率を測定する具体的な指標はありませんが、KEPはコミュニティから大きな関心を集めています。特にIstioのようなサービスメッシュベンダーは、アルファテストフェーズに積極的に参加しました。KEPの可視性は、多数のブログ投稿、インタビュー、講演、ワークショップを通じてさらに実証されています。KEPは、ネットワークプロキシ、ロギングシステム、セキュリティ対策など、KubernetesのPod内のメインコンテナと並行して追加機能を提供する需要の増加に対応しています。コミュニティは、この機能の広範な採用を促進するために、既存のワークロードに対する容易な移行パスを提供することの重要性を認識しています。

**Arpit:** 本番環境でサイドカーコンテナを使用している企業の注目すべき例や成功事例はありますか？

**M/G/S:** 本番環境での広範な採用を期待するにはまだ早すぎます。1.29リリースは2024年1月11日からGoogle Kubernetes Engine(GKE)で利用可能になったばかりで、ユニバーサルインジェクターを介して効果的に有効化し使用する方法に関する包括的なドキュメントがまだ必要です。人気のあるサービスメッシュプラットフォームであるIstioも、ネイティブサイドカーを有効にするための適切なドキュメントが不足しているため、開発者がこの新機能を使い始めるのが難しくなっています。しかし、ネイティブサイドカーのサポートが成熟し、ドキュメントが改善されるにつれて、本番環境でのこの技術のより広範な採用が期待できます。

**Arpit:** 提案では、サイドカー機能を実現するために初期化したコンテナに`restartPolicy`フィールドを導入することが示されています。この方法で、先ほど挙げられた課題をどのように解決できるのか、詳しく教えていただけますか？

**M/G/S:** 初期化したコンテナに`restartPolicy`フィールドを導入する提案は、既存のインフラストラクチャを活用し、サイドカーの管理を簡素化することで、概説された課題に対処します。このアプローチは、Podの仕様に新しいフィールドを追加することを避け、管理しやすさを保ちつつ、さらなる複雑さを回避します。既存の初期化したコンテナのメカニズムを利用することで、サイドカーはPodの起動時に通常の初期化コンテナと並行して実行でき、一貫した初期化の順序を確保します。ささらに、サイドカー用の初期化コンテナの再起動ポリシーを`Always`に設定することで、メインアプリケーションコンテナが終了した後も、ロギングやモニタリングなどの継続的なサービスをワークロードの終了まで維持できます。

**Arpit:** 初期化したコンテナに`restartPolicy`フィールドを導入することは、既存のKubernetes設定との後方互換性にどのような影響を与えますか？

**M/G/S:** 初期化したコンテナに`restartPolicy`フィールドを導入しても、既存のKubernetes設定との後方互換性は維持されます。既存の初期化したコンテナは従来通りに機能し続け、新しい`restartPolicy`フィールドは、明示的にサイドカーとして指定された初期化したコンテナにのみ適用されます。このアプローチにより、既存のアプリケーションやデプロイメントが新機能によって中断されることはなく、同時にサイドカーをより効果的に定義および管理する方法が提供されます。

## SIG Nodeへの貢献

**Arpit:** 新しいメンバー、特に初心者が貢献するのに最適な方法は何でしょうか？

**M/G/S:** 新しいメンバーや初心者は、サイドカーに関するKEP(Kubernetes Enhancement Proposal)に対して、以下の方法で貢献できます:

- **認知度の向上:** サイドカーの利点と使用例を紹介するコンテンツを作成します。これにより、他の人々にこの機能の理解を深めてもらい、採用を促すことができます。
- **フィードバックの提供:** サイドカーの使用経験(良い点も悪い点も)を共有してください。このフィードバックは、機能の改善や使いやすさの向上に役立ちます。
- **ユースケースの共有:** 本番環境でサイドカーを使用している場合は、その経験を他の人と共有してください。実際の使用例を示すことで、この機能の価値を実証し、他の人々の採用を促進できます。
- **ドキュメントの改善:** この機能のドキュメントの明確化や拡充にご協力ください。より分かりやすいドキュメントは、他の人々がサイドカーを理解し、活用する助けになります。

サイドカーに関するKEP以外にも、SIG Nodeではより多くの貢献者を必要としている分野があります:

- **テストカバレッジの向上:** SIG Nodeでは、Kubernetesコンポーネントのテストカバレッジを継続的に改善する方法を模索しています。

- **CI(継続的インテグレーション)の維持:** SIG Nodeは、Kubernetesコンポーネントが様々な状況下で期待通りに動作することを確認するため、一連のエンドツーエンド(e2e)テストを管理しています。

# 結論

SIG Nodeは、Kubernetesの発展において重要な役割を果たしています。
クラウドネイティブ・コンピューティングの絶えず変化する環境の中で、Kubernetesの信頼性と適応性を確保し続けています。
Matthias、Gunju、Sergeyといった献身的なメンバーが先頭に立ち、SIG Nodeは革新の最前線に立ち続けています。
彼らの努力により、Kubernetesは新たな地平を目指して前進し続けているのです。
