# Slack アプリ台帳

aileron-inc/slack-apps は Slack App のマニフェストと手順の置き場。Events Worker は [aileron-inc/slack-support-bypass](https://github.com/aileron-inc/slack-support-bypass)。返信ロジックと Bot Token は Grok Bot 側。

App ID の正は、オーナーが貼った Slack「Your Apps」一覧（2026-10-07）。ワークスペース名もその一覧の表記（Cajon,Inc. / XTalent / Aileron inc）。team ID `T9U503RME` と `TMGCWC4TY` は、このリポジトリに以前から書いてあった値。Your Apps の貼り付けには team ID が無い。Aileron inc の team ID は未記載。

Your Apps を台帳にした時点では Slack API を呼んでいない。Event URL の実ホストは確認していない。Request URL は `https://<bypass-worker>` + path で、hostname は書かない。

2026-10-07（JST）に `slack app delete --force` で 8 件の App ID を消した。このフォローアップでは Slack API を呼んでいない。削除と再作成の記録はオーナーの実施報告。

withwork-bot の旧 ID `A041U5XSVQQ` は誤削除。retire-candidate ではなく、マル之進の後継でもない。同日 `A0C84BDDDHN` として再作成した。それ以外の 7 件は削除のまま。

| 分類 | 意味 |
| --- | --- |
| keep-active | 稼働中の Event bot（bypass）。マニフェストをこのリポジトリに置く |
| product-bot | フォーム / interactivity の本番 bot。Events → bypass ではない |
| deleted | 2026-10-07 に削除した旧 App ID。withwork-bot の旧 ID は誤削除で同日再作成済み。それ以外は再作成しない |
| retired | 後継が決まっている廃止アプリの記録。月島灯の Slack アプリ自体は deleted |
| dormant | 残っているが、いまの受信パスとしては使っていない |
| keep-elsewhere | Cajon / XTalent のその他プロダクトアプリ。作り直さない |
| personal/demo | 個人アプリ。Demo App は deleted |

## keep-active（マニフェストはここ）

| アプリ名 | App ID | slug | path | ワークスペース |
| --- | --- | --- | --- | --- |
| [さえ｜Hairbookサポート](apps/sae/README.md) | `A0BUXNTB43U` | sae | `/slack/hairbook` | Cajon,Inc. |
| [森崎 紬｜サロンジョブズ](apps/tsumugi/README.md) | `A0BUXEUUPM0` | tsumugi | `/slack/salonjobs` | Cajon,Inc. |
| [日向 ひなた｜シンサロンページ](apps/hinata/README.md) | `A0BUXV96DM0` | hinata | `/slack/synsalon` | Cajon,Inc. |
| [マル之進 (マルノシン)](apps/withwork/README.md) | `A0C2DNVFPK8` | withwork | `/slack/withwork` | XTalent |

さえの bot user は 2026-09-17 の `auth.test` で `hairbook_support`。マル之進の bot `display_name` は `marunoshin`。日向のライブ @mention は Your Apps に無い。紬の manifest は bypass 最小より広いスコープを export から残している。

この 4 件は 2026-10-07 の削除に入っていない。`A0BUXNTB43U`、`A0BUXEUUPM0`、`A0BUXV96DM0`、`A0C2DNVFPK8` は稼働のまま。マル之進は withwork-bot とは別システム。シークレットのキーは `*_WITHWORK`。

## product-bot（Events / bypass ではない）

| アプリ名 | App ID | bot | ワークスペース | 役割 |
| --- | --- | --- | --- | --- |
| [withwork-bot](apps/withwork-bot/README.md) | `A0C84BDDDHN` | `@withwork`（`B0C6UJMAR2B` / `U0C781KG92A`） | XTalent | フォーム投稿とランク/CA の interactivity。CF Worker `withwork-slack-sync` と Rails の `SLACK_API_TOKEN`。Event Subscriptions は bypass に向けない。マル之進 `A0C2DNVFPK8` とは別 |

## Deleted 2026-10-07

2026-10-07（JST）、`slack app delete --force`。月島灯、Hairbook Support、紬の local と同名、Mia local、Demo App、withworkコロ助は削除のまま。withwork-bot の旧 ID だけは誤削除で、同日再作成した。

| アプリ名 | App ID | ワークスペース | メモ |
| --- | --- | --- | --- |
| [月島 灯｜Hairbookサポート](apps/akari/README.md) | `A0BJYNERNG6` | Cajon,Inc. | Socket Mode の旧アプリ。後継は さえ `A0BUXNTB43U`。記録は `apps/akari` |
| Hairbook Support | `A0BUVGCE47Q` | Cajon,Inc. | 英語名の旧サポート。後継は さえ `A0BUXNTB43U` |
| 森崎 紬｜サロンジョブズ (local) | `A0BUR8478TV` | Cajon,Inc. | 本番 `A0BUXEUUPM0` の local 重複 |
| 森崎 紬｜サロンジョブズ | `A0BUGFXQNDD` | Cajon,Inc. | 本番と同じ表示名の別アプリ。bypass 本番は `A0BUXEUUPM0` |
| アシスタント Mia (local) | `A0BV0BC2U2C` | Cajon,Inc. | Mia `A0BUQDC87BN` の local 重複 |
| Demo App | `A0C2ANFQYUF` | Cajon,Inc. | デモ |
| withworkコロ助 | `A0C20A95WR5` | XTalent | 旧表示名。ライブの Event bot は マル之進 `A0C2DNVFPK8` |
| withwork-bot（旧） | `A041U5XSVQQ` | XTalent | 誤削除。マル之進の後継ではない。同日再作成 `A0C84BDDDHN`。旧 bot `B048NKTU1JL` / user `U047W0N0DPX` は履歴 |

## retired

月島灯の Socket Mode 記録は [apps/akari](apps/akari/README.md) に残す。`manifest.legacy.yml` は貼らない。Slack アプリ `A0BJYNERNG6` は上の Deleted 2026-10-07 に含む。

## dormant

| アプリ名 | App ID | ワークスペース | メモ |
| --- | --- | --- | --- |
| cmd | `A08QKUREZK2` | Cajon,Inc. | アーカイブ済みの hairbook-slack-cmd とみる。受信パスとしては使っていない |

## keep-elsewhere（記録のみ。作り直さない）

マニフェストは置かない。コードも移さない。

| アプリ名 | App ID | ワークスペース | メモ |
| --- | --- | --- | --- |
| Raptor | `A0BQ58HJDLM` | Cajon,Inc. | `cajon-inc/raptor-bot` |
| Mia | `A0BUQDC87BN` | Cajon,Inc. | プロダクトの Mia。local 重複 `A0BV0BC2U2C` は 2026-10-07 に削除済み |
| cajon-bot | `A06LCPZTYLX` | Cajon,Inc. | noroshi-flash などを含むプロダクト bot |
| ノロシ | `A0C0N6Q96U8` | Cajon,Inc. | noroshi |
| sms-support | `A0C5DJ4NCJZ` | Cajon,Inc. | プロダクト / サポート |
| ai | `A0A6PBHPW8N` | XTalent | XTalent の ai |
| lumi | `A0BQZKEGQGL` | Cajon,Inc. | 記録のみ。置き場のリポジトリ名は Your Apps のメモに無い |
| 質問ちゃん | `A0C2ZPZG2P8` | Cajon,Inc. | Slack App がある（Grok MCP だけではない）。Events→Grok にするならマニフェストは別判断。この PR では置かない |
| カク之進 (カクノシン) | `A0C3LF4FNGY` | Cajon,Inc. | Slack App がある。MCP teammate がこの bot を使う可能性はオーナーメモ。この PR ではマニフェストを置かない |

## personal/demo

| アプリ名 | App ID | ワークスペース | メモ |
| --- | --- | --- | --- |
| masa-dot | `A0C6GJ0LYUT` | Aileron inc | 個人アプリ。Demo App `A0C2ANFQYUF` は Deleted 2026-10-07 |

## この Your Apps 一覧に無い名前

以前の相談メモにだけある。App ID は割り当てない。

| 名前 | メモ |
| --- | --- |
| magazine-review-bot | Your Apps に無い |
| mikin-daily-bot | Your Apps に無い |
| lead-status-bot | Your Apps に無い |
| incident-patrol | Your Apps に無い |
| withwork-slack-webhook | Your Apps にこの名前は無い。XTalent の Event bot は マル之進 `A0C2DNVFPK8`。フォーム bot は withwork-bot `A0C84BDDDHN`。コロ助 `A0C20A95WR5` は Deleted 2026-10-07 |
| 柚希あかり（Grok） | Your Apps に無い。カク之進と質問ちゃんは Slack App として上表にある |
| `cajon-inc/hairbook-runbook-bot` | Your Apps の行には無い。日向のローカルメモが返信実装として指していた。Slack アプリの行は 日向 `A0BUXV96DM0` |

## ローカルクローンの重複

| パス | 何か | 正本 |
| --- | --- | --- |
| `~/Projects/slack-apps` | git 管理ではないローカルハブ（旧 README、アイコン、secrets へのリンク） | このリポジトリ（aileron-inc/slack-apps） |
| `~/Projects/slack-support-bypass` | 削除済み `cajon-inc` リモートを向いた orphan クローン | 下の行 |
| `~/Projects/slack-support-bypass-aileron` | Events Worker のクローン | [aileron-inc/slack-support-bypass](https://github.com/aileron-inc/slack-support-bypass) |

この cloud agent からは `~/Projects` を見ていない。上の 3 行は相談結果としての記録。ハブの `secrets` や `routes.json`、Bot Token は git に入れない。
