# Helm

- Helm works as a package manager (package manager enable you install, uninstall, upgrade, ... easily. Some common package managers: apt, homebrew, Choclatey) with Kubenrtes containerized applications. It removes the headacke of running each YAML file and the burden of updating them. It provides a lot more functionalities can be discovered through kodekloud course.
- Charts are files contain all the data needed to create releases and revisions in the cluster. There are a lot of published built-in charts, then you need just to update and configure them. All published charts can be found on ArtifactHub (helm charts hub)

        # helm install [release name] [chart name]

- Charts mainly composed from three type of YAML files:

  1. Template: the yamale files of that create services and deployments, etc.
  2. Values: a YAML file that contains the values that referenced in template files.
  3. Chart: a YAML file used to save information about the chart.

- To create a chart run the following command to get the main skaleton of the chart:

        $ helm create [the name of the chart]

- To make the templates flexible you have to use go templating lanuage to define the the name of the template to make it avialable to use by any number of releases. The go template define as following:

        {{.Rlease.Name}}-nginx
        ### Notice that there are a lot of other variable that you can set their values
        ### To set a default value:
        {{default "nginx".Values.image.repository}}-nginx
        ### Also you can use pipes with functions to add morfe functions:
        {{ .Values.image.repository | default "nginx"}}-nginx

- To use conditional statements in YAML the n use the following:

        {{- if .values.x}}
        labels:
        ...
        {{- else if eq .values.x "love"}}
        labels:
        ...
        {{- end}}

- Notice that the root scope refered as `$` and the root has two main subspaces `Release` and `values` you can set narrower subspaces as following:

        {{- with Values.app}}
        labels:
            tier: {{title}}
        {{- end}}

- If there is variable in the values file is a list then you have to iterate on its elements in the template files whenever you want to use these values:

        cars:
        {{- range values.carList}}
        - {{ . | quote}}
        {{- end}}

- To reduce the repetetion in the templates you can use helper files. Helper files

- To test the format of the chart and show the format errors if exist:

        $ helm lint [the chart folder path]

- To validate the templates and make sure that all the values in the templates are replaced with the proper values use the following command:

        # use --debug flag to see the error location.
        $ helm template [the chart folder path]

- To discover the kubrnetes manifest format errors use the following command:

        $ helm install [release name] [path of the charts file] --dry-run

- To search about a chart in Artifacthub you can use the following command:

        $ helm search hub [keyword]

- To deploy code using helm use the following commands:

        $ helm repo add [name of the chart] [ the link of the chart]
        ## You can update any value in the Values.yaml file using --set flag
        $ helm install --set [variable name]="value" --set [variable 2]="value 2" [release name] [the name of the chart]

- To apdate the values in the Values.yaml file using a custom values file:

        $ helm install --values custom=file.yaml [release name] [the name of the chart]

- To uninstall a release use the following command:

        $ helm uninstall [release name]

- Another way to install charts and install the releases:

        # First pull the the chart
        $ helm pull [chart name]
        # Second uncompress the file, thn update Values.yaml file
        $ helm pull --untar [chart name]
        # Third install from the file directory
        $ helm install [release name] [the directory of the uncompressed folder]

- To get all the save chart templates:

        $ helm repo list
        # To update a charts repos
        $ helm repo update

- To get all the app's releases:

        $ helm list
        # to get more details
        $ helm history [release name]

- To upgrade a release you can use the following command and that will create a new revision of the release:

        $ helm upgrade [release name] [the link of the new upgrade]

- To return back to an older version:

        $ helm rollback [release name] [the revision number]
