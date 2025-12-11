# -----------------------------------------------------
# Advanced Graphviz Makefile
# -----------------------------------------------------

STYLE = style.doth
CPP = cpp -P -I.
DOT = dot

# -----------------------------------------------------
# 1. ファイル探索 (再帰的)
# -----------------------------------------------------
# findコマンドを使って，サブディレクトリを含む全.dotファイルを取得
SRCS = $(shell find . -type f -name "*.dot")

# 拡張子なしのリスト (sato/hoge, kato/fuga ...)
BASENAMES = $(SRCS:.dot=)

# 全ファイルの成果物リスト
ALL_PNGS = $(SRCS:.dot=.png)
ALL_SVGS = $(SRCS:.dot=.svg)

# -----------------------------------------------------
# 2. モード切替ロジック (ここがポイント)
# -----------------------------------------------------

# デフォルトは「両方作る」
TARGET_EXTS = .png .svg

# もしコマンド引数に "svg" が含まれていたら...
ifneq ($(filter svg,$(MAKECMDGOALS)),)
    # ターゲットの拡張子を .svg だけにする
    TARGET_EXTS = .svg
    # "svg" というターゲット自体は何もしない (ダミーにする)
    RUN_SVG_TASK = @:
else
    # 通常時は "make svg" で全SVGを作るタスクを割り当てる
    RUN_SVG_TASK = $(MAKE) -f $(lastword $(MAKEFILE_LIST)) all_svgs
endif

# もしコマンド引数に "png" が含まれていたら...
ifneq ($(filter png,$(MAKECMDGOALS)),)
    TARGET_EXTS = .png
    RUN_PNG_TASK = @:
else
    RUN_PNG_TASK = $(MAKE) -f $(lastword $(MAKEFILE_LIST)) all_pngs
endif

# -----------------------------------------------------
# 3. ルール定義
# -----------------------------------------------------

# デフォルト (make): 全ファイルの全形式を作成
all: all_pngs all_svgs

# バッチ処理用ターゲット (内部呼び出し用)
all_pngs: $(ALL_PNGS)
all_svgs: $(ALL_SVGS)

# キーワードターゲットの定義
.PHONY: svg png
svg:
	$(RUN_SVG_TASK)
png:
	$(RUN_PNG_TASK)

# -----------------------------------------------------
# 4. 個別ファイル用ショートカット
# -----------------------------------------------------

# "sato/hoge" と打たれたとき，現在のモード(TARGET_EXTS)に合わせて依存関係を変える
.PHONY: $(BASENAMES)
$(BASENAMES): %: $(addprefix %,$(TARGET_EXTS))

# -----------------------------------------------------
# 5. コンパイルレシピ
# -----------------------------------------------------

%.png: %.dot $(STYLE)
	@echo "Compiling [PNG] $<"
	@mkdir -p $(dir $@)
	$(CPP) $< | $(DOT) -Tpng -o $@

%.svg: %.dot $(STYLE)
	@echo "Compiling [SVG] $<"
	@mkdir -p $(dir $@)
	$(CPP) $< | $(DOT) -Tsvg -o $@