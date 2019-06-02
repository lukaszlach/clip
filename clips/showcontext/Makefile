PLUGIN_COMMAND ?= showcontext
PLUGIN_IMAGE ?= lukaszlach/clips:${PLUGIN_COMMAND}

build:
	docker clip build -c ${PLUGIN_COMMAND} -t ${PLUGIN_IMAGE} .

install:
	docker clip add ${PLUGIN_IMAGE}

rm:
	docker clip rm ${PLUGIN_COMMAND}

reinstall: rm install

full-reinstall: build reinstall