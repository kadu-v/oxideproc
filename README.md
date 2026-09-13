# oxideproc

`cuda-oxide` を使った GPU 前処理ライブラリの開発用プロジェクトです。現在は環境確認用として、1,024 要素のベクトル加算カーネルを収録しています。

`cuda-oxide` と `cargo-oxide` はコミット `6abfaa091e29a6275c1943895bfbc97efa306e98` に固定しています。

## 必要なホスト環境

- Linux
- Compute Capability 8.0 以降の NVIDIA GPU
- CUDA 13.0 と互換性のある NVIDIA Driver
- Docker と NVIDIA Container Toolkit
- VS Code の Dev Containers 拡張（VS Code から利用する場合）

CUDA Toolkit、LLVM、Clang、Rust はコンテナ内にインストールされるため、ホストへの導入は不要です。

## VS Code で起動

このフォルダーを VS Code で開き、コマンドパレットから `Dev Containers: Reopen in Container` を実行します。初回はイメージと `cargo-oxide` のビルドに時間がかかります。

コンテナ内のターミナルで環境を確認します。

```bash
cargo oxide doctor
```

GPU カーネルをビルドして実行します。

```bash
cargo oxide run
```

成功時は次のように表示されます。

```text
PASSED: all 1024 elements correct
```

## 依存を更新する場合

`cuda-device`、`cuda-host`、Dockerfile 内の `cargo-oxide` は必ず同じ Git コミットへ更新してください。そのコミットの `rust-toolchain.toml` に合わせて、このプロジェクトと Dockerfile の nightly も同時に更新します。
