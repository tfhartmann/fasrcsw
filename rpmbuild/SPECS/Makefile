.PHONY: all trial build filelist filequery test install query post uninstall

all: build

trial:
	fasrcsw-rpmbuild-$$TYPE --define 'trial yes' -ba "$$NAME-$$VERSION-$$RELEASE".spec

build:
	fasrcsw-rpmbuild-$$TYPE -ba "$$NAME-$$VERSION-$$RELEASE".spec

filelist:
	fasrcsw-list-$$TYPE-rpms "$$NAME-$$VERSION-$$RELEASE"

filequery:
	fasrcsw-rpm -qilp $$(fasrcsw-list-$$TYPE-rpms "$$NAME-$$VERSION-$$RELEASE")

test:
	sudo -E fasrcsw-rpm -ivh --oldpackage --nodeps --test $$(fasrcsw-list-$$TYPE-rpms "$$NAME-$$VERSION-$$RELEASE")

install: 
	sudo -E fasrcsw-rpm -ivh --oldpackage --nodeps $$(fasrcsw-list-$$TYPE-rpms "$$NAME-$$VERSION-$$RELEASE")
	sudo -E fasrcsw-update-lmod-spider-cache

query:
	fasrcsw-rpm -qa | grep "^$$NAME-$$VERSION-$$RELEASE" || true

post:
	sudo rsync -avu {"$$FASRCSW_DEV","$$FASRCSW_PROD"}/rpmbuild/SOURCES/
	sudo rsync -avu {"$$FASRCSW_DEV","$$FASRCSW_PROD"}/rpmbuild/RPMS/
	sudo rsync -avu {"$$FASRCSW_DEV","$$FASRCSW_PROD"}/rpmbuild/SRPMS/
	cd "$$FASRCSW_DEV"
	git add --all .
	git commit -v .
	git pull
	git push

uninstall: 
	sudo -E fasrcsw-rpm -ev --nodeps $$(fasrcsw-list-$$TYPE-rpms "$$NAME-$$VERSION-$$RELEASE" | xargs -I% basename % .rpm)
	sudo -E fasrcsw-update-lmod-spider-cache
