# -----------------------------------------------------
# Smart Graphviz Makefile
# -----------------------------------------------------

STYLE = style.doth
CPP = cpp -P
DOT = dot
DOT_FLAGS = -Tpng

# ディレクトリ内の全.dotファイルを取得
SRCS = $(wildcard *.dot)
# "main" や "flow" のように拡張子を除いたリストを作る
TARGETS_NOEXT = $(SRCS:.dot=)

# デフォルトですべてコンパイル
all: $(SRCS:.dot=.png)

# -----------------------------------------------------
# マジックルール
# -----------------------------------------------------

# 1. これにより "make main" と打てるようになる
#    (main は main.png に依存するという定義)
.PHONY: $(TARGETS_NOEXT)
$(TARGETS_NOEXT): %: %.png

# 2. 実際のコンパイルルール (main.png は main.dot から作る)
%.png: %.dot $(STYLE)
	@echo "Compiling $< ..."
	$(CPP) $< | $(DOT) $(DOT_FLAGS) -o $@

.PHONY: all clean