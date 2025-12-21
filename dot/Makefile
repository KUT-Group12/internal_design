# -----------------------------------------------------
# Advanced Graphviz Makefile
# -----------------------------------------------------

STYLE = style.doth
CPP = cpp -P -I.
DOT = dot

# -----------------------------------------------------
# 0. ディレクトリ指定の解釈
# -----------------------------------------------------

TARGET_DIRS = $(filter-out svg png all,$(MAKECMDGOALS))

.PHONY: $(TARGET_DIRS)
$(TARGET_DIRS): ;


# -----------------------------------------------------
# 1. ファイル探索 (再帰的)
# -----------------------------------------------------

# デフォルト（従来どおり全探索）
SRCS = $(shell find . -type f -name "*.dot")

# ディレクトリが指定されていれば，その配下のみ探索（上書き）
ifneq ($(strip $(TARGET_DIRS)),)
SRCS = $(shell find $(TARGET_DIRS) -type f -name "*.dot")
endif

# 拡張子なしのリスト
BASENAMES = $(SRCS:.dot=)

# 全ファイルの成果物リスト
ALL_PNGS = $(SRCS:.dot=.png)
ALL_SVGS = $(SRCS:.dot=.svg)

# -----------------------------------------------------
# 2. モード切替ロジック（完全にそのまま）
# -----------------------------------------------------

TARGET_EXTS = .png .svg

ifneq ($(filter svg,$(MAKECMDGOALS)),)
    TARGET_EXTS = .svg
    RUN_SVG_TASK = @:
else
    RUN_SVG_TASK = $(MAKE) -f $(lastword $(MAKEFILE_LIST)) all_svgs
endif

ifneq ($(filter png,$(MAKECMDGOALS)),)
    TARGET_EXTS = .png
    RUN_PNG_TASK = @:
else
    RUN_PNG_TASK = $(MAKE) -f $(lastword $(MAKEFILE_LIST)) all_pngs
endif

# -----------------------------------------------------
# 3. ルール定義（完全にそのまま）
# -----------------------------------------------------

all: all_pngs all_svgs

all_pngs: $(ALL_PNGS)
all_svgs: $(ALL_SVGS)

.PHONY: svg png
svg: all_svgs ;


png: all_pngs ;


# -----------------------------------------------------
# 4. 個別ファイル用ショートカット（そのまま）
# -----------------------------------------------------

.PHONY: $(BASENAMES)
$(BASENAMES): %: $(addprefix %,$(TARGET_EXTS))

# -----------------------------------------------------
# 5. コンパイルレシピ（そのまま）
# -----------------------------------------------------

%.png: %.dot $(STYLE)
	@echo "Compiling [PNG] $<"
	@mkdir -p $(dir $@)
	$(CPP) $< | $(DOT) -Tpng -o $@

%.svg: %.dot $(STYLE)
	@echo "Compiling [SVG] $<"
	@mkdir -p $(dir $@)
	$(CPP) $< | $(DOT) -Tsvg -o $@
