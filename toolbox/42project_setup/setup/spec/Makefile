# Please only change the following line
TARGET_NAMES=

NAME=debug.out
TARGET_DIR=../src
TESTS:=$(shell find tests -name '*.c')
TARGETS=${addprefix ${TARGET_DIR},${TARGET_NAMES}}
TESTS_OBJS=${TESTS:.c=.o}
CC=cc
CFLAGS=-Wall -Werror -Wextra -Wformat
CFLAGS+= -fsanitize=address -g3

all: ${NAME}

${NAME}: targetall ${TESTS_OBJS}
ifeq ($(findstring .a, ${TARGETS}),)
	@${CC} ${CFLAGS} main.c ${TESTS_OBJS} ${TARGETS:.c=.o} -o ${NAME} && \
	echo "Success compiling with the target binary ${TARGET_NAMES}!"
else
	@${CC} ${CFLAGS} main.c ${TESTS_OBJS} -L./${TARGET_DIR} -l$(patsubst lib%.a,%,${TARGET_NAMES}) -o ${NAME} && \
	echo "Success compiling with the target library ${TARGET_NAMES}!"
endif

%.o: %.c
	@${CC} ${CFLAGS} -c $< -o $@

clean: targetclean
	@rm -f ${TESTS_OBJS}

fclean: targetfclean clean
	@rm -f ${NAME}

re: fclean all

# Target rules

targetall:
	@make CFLAGS=${CFLAGS} re -C ${TARGET_DIR}

targetclean:
	@make clean -C ${TARGET_DIR}

targetfclean:
	@make fclean -C ${TARGET_DIR}