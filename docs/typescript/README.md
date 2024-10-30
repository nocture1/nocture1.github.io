# TypeScript学習メモ

## 基本
JavaScriptをベースに静的型付け＋オブジェクト指向を盛り込んだ言語

## TypeScript選択のモチベーション
- JavaScriptに型付けを行える
    + 互換性を持たない型に値を割り当てることができない

JavaScriptなら実行時に型の検証が行われるが、事前に行うことで悪い芽を早期に摘める。  
ただし、開発者の成熟度によってはJavaScriptのままで進むのが安全なこともあるはず。

TypeScriptを利用する上で、 `any` （動的型付けとして振る舞う型）に対する処方箋でなく、  
`any` を緩やかに活用するためのロードマップを記す。

## [基本編](https://nocture1.github.io/typescript/基本編)
- 型安全な条件分岐の基本パターンを理解する。

## [初歩編](https://nocture1.github.io/typescript/初心編)
- 基本編では対応できない複雑なパターンに対応する。
- 今まで定義してきた型を再利用する。

## ステップアップ編
- 型に仕様や制約を込めることで、Primitive Obsessionから脱却する。
