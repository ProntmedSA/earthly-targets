ARG GITVERSION_DOCKER_TAG=5.8.0-alpine.3.14-6.0-amd64

CSHARP_COPY_RESTORE:
    COPY . .
    RUN find . -name "*.sln" -prune -o \! -type d \! -name \*.csproj \! -name \*.csproj -exec rm -f '{}' + \
    && find . -depth -type d -empty -exec rmdir '{}' \;

gitversion:
	FROM gittools/gitversion:$GITVERSION_DOCKER_TAG
	COPY ./.git /repo/.git
    COPY --if-exists GitVersion.yml /repo/GitVersion.yml
	RUN /tools/dotnet-gitversion /repo /showvariable FullSemVer > /repo/fullsemver
	RUN /tools/dotnet-gitversion /repo /showvariable NuGetVersionV2 > /repo/nugetver
	RUN cat /repo/nugetver | xargs echo "The current version is $@"
    SAVE ARTIFACT /repo/fullsemver /fullsemver
	SAVE ARTIFACT /repo/nugetver /nugetver