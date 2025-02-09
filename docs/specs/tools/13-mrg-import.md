---
id: mrg-import
sidebar_label: MRG Import
date: 20230731
---

# MRG Import Tool

import useBaseUrl from '@docusaurus/useBaseUrl'
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

<!-- Use 'Mark' as an HTML tag, e.g. <mark>text to mark</Mark?-->
export const mark = ({children}) => (
  <span style={{ color:'black', backgroundColor:'yellow', padding:'0.2rem', borderRadius:'2px', }}>
    {children}
  </span> );

The **[MRG](@) Import Tool ([MRG importer](@))** ensures that the [scope](@) within which it is run, obtains a local copy of all [MRGs](@) that are available in the [scopes](@) that are mentioned in the [`scopes` section](/docs/specs/files/saf#scopes) of its [SAF](@). This makes life easy for various tools, e.g., the [MRGT](@) and the [TRRT](@), that can now assume that all [MRGs](@) that they may need to consult in order to do their job, are readily available.

## MRG files

This means concretely that within the [glossarydir](@) of a [scope](@) that has run the [MRG importer](@), the following files are available for every [scopetag](@) `st` that exists in the [`scopes` section](/docs/specs/files/saf#scopes) of its [SAF](@):

- `mrg.st.<vsntag>.yaml` contains the actual [MRG entries](@) for the [terminology](@) specified in the [`versions`-section](/docs/specs/files/saf#versions) whose `vsntag` field contains `<vsntag>`.
- `mrg.st.<altvsntag>.yaml` is a copy of the `mrg.st.<vsntag>.yaml` file  where `<altvsntag>` is one of the alternative [versiontags](@) by which the [MRG](@) can be referenced.[^1]
- `mrg.st.yaml` is a copy of the `mrg.st.<vsntag>.yaml` file  where `<vsntag>` is the value of the `defaultvsn`-field in the [`scope`-section](/docs/specs/files/saf#scope-section) of the [SAF](@) of [scope](@) `st`.

[^1]: Previous versions of the specifications said this would be a symbolic link to the [MRG](@) rather than a copy thereof. However, symbolic links created for the purpose of functioning in a (Git) repo would not work in a local development context (e.g. on a Windows machine), and vice versa. To remedy this, and taking into consideration that [MRGs](@) are relatively small in size, made us decide to use actual copies. Note that you can still see which files are copies by inspecting the first section of the [MRGs](@), which lists the [versiontag](@) and the `altvsntags` (alternative [versiontags](@) that can be used) of the [terminology](@) that the [MRG](@) documents.

## Installing the Tool

The tool can be installed from the command line and made globally available by executing

~~~
npm install -g @tno-terminology-design/mrg-import
~~~

<details>
  <summary>Before running the tool from the command line, make sure you have met the necessary prerequisites depending on your operating environment.</summary>

<Tabs
  defaultValue="cmd.exe"
  values={[
    {label: 'CMD.exe (Windows)', value: 'cmd.exe'},
    {label: 'PowerShell(Windows)', value: 'powershell'},
    {label: 'Bash (Linux/Mac)', value: 'bash'},
  ]}>

<TabItem value="cmd.exe"><br/>

1. **Node.js and NPM**: Ensure Node.js and NPM are installed.
2. **Global Installation**: If you have installed the package globally, confirm the global NPM modules path by running `npm config get prefix`. The global modules are usually stored under `<prefix>/node_modules`.
3. **Environment Variables**: Add the path to global NPM binaries to your system's PATH environment variable. This should be `<prefix>` on Windows. To add to PATH, you can edit your environment variables or run `set PATH=%PATH%;<prefix>` in the CMD.

</TabItem>
<TabItem value="powershell"><br/>

1. **Node.js and NPM**: Ensure Node.js and NPM are installed.
2. **Global Installation**: Check the global NPM modules path as in CMD.
3. **Environment Variables**: Update the PATH environment variable as in CMD. You can also use `$env:Path += ";<prefix>"` to update the PATH temporarily in the current PowerShell session.

</TabItem>
<TabItem value="bash"><br/>

1. **Node.js and NPM**: Ensure Node.js and NPM are installed.
2. **Global Installation**: If globally installed, run `npm config get prefix` to get the global modules path, usually `<prefix>/lib/node_modules`.
3. **Environment Variables**: Add the `<prefix>/bin` directory to your `PATH` if it's not already. You can do this by adding `export PATH=$PATH:<prefix>/bin` to your `~/.bashrc` or `~/.zshrc` file.

</TabItem>
</Tabs>
</details>

## Calling the Tool

The behavior of the [MRG importer](@) can be configured per call e.g. by a configuration file and/or command-line parameters. The command-line syntax is as follows:

~~~
mrg-import [ <paramlist> ]
~~~

where `<paramlist>` is an (optional) list of parameters.

<details>
  <summary>Legend</summary>

The columns in the following table are defined as follows:
1. **`Parameter`** specifies the parameter and further specifications
2. **`Req'd`** specifies whether (`Y`) or not (`n`) the field is required to be present when the tool is being called. If required, it MUST either be present in the configuration file, or as a command-line parameter.
3. **`Description`** specifies the meaning of the `Value` field, and other things you may need to know, e.g. why it is needed, a required syntax, etc.

If a configuration file used, the long version of the parameter must be used (without the preceding `--`).
</details>

| Key                          | Req'd | Description |
| :--------------------------- | :---: | :---------- |
| `-c`, `--config <path>`        | n | Path (including the filename) of the tool's (YAML) configuration file. |
| `-s`, `--scopedir <path>`      | Y | Path of the scope directory from which the tool is called. |
| `-o`, `--onNotExist <action>`  | n | The action in case an MRG file unexpectedly does not exist. |
| `-V`, `--version`              | n | output the version number of the tool. |
| `-h`, `--help`                 | n | display help for command. |

The `<action>` parameter can take the following values:

| `<action>` | Description |
| :--------- | :---------- |
| `'throw'`  | an error is thrown (an exception is raised), and processing will stop. |
| `'warn'`   | a message is displayed (and logged) and processing continues. |
| `'log'`    | a message is written to a log(file) and processing continues. |
| `'ignore'` | processing continues as if nothing happened. |

## Processing, Errors and Warnings

The [MRG importer](@) starts by reading its command-line and configuration file. If the command-line has a key that is also found in the configuration file, the command-line key-value pair takes precedence. The resulting set of key-value pairs is tested for proper syntax and validity. Every improper syntax and every invalidity found will be logged. Improper syntax may be e.g. an invalid [globpattern](https://en.wikipedia.org/wiki/Glob_(programming)#Syntax). Invalid conditions include non-existing directories or files, lack of write-permissions where needed, etc.

Then, the [MRG importer](@) reads the [SAF](@) of the [scope](@) from which the [MRG importer](@) is called. We will use the following names for values that are in the [SAF](@):

- `{my-own-scopetag}` = `scopetag`-field from the `scope`-section
- `{my-own-scopedir}` = `scopedir`-field from the `scope`-section
- `{my-own-glossarydir}` = `glossarydir`-field from the `scope`-section

The [MRG importer](@) also reads the [`scopes` section](/docs/specs/files/saf#scopes) of the [SAF](@), which specifies the 'other' [scopes](@) from which the actively maintained [MRGs](@) have to be imported. This [`scopes` section](/docs/specs/files/saf#scopes) contains elements that consist of two parts, whose values we will refer to by the following names:

- `{import-scopetag}` = `scopetag`-field from the `scopes`-section of the [SAF](@)
- `{import-scopedir}` = `scopedir`-field from the `scopes`-section of the [SAF](@)

For every `{import-scopedir}`, the [MRG importer](@) will read its [SAF](@) to find out which [terminologies](@) are being actively maintained (we will use `{import-saf}` to refer to the contents of this [SAF](@)).

:::info Editor's note
Reading a SAF may require authentication, e.g. when the scopedir of the other scope is in a private or enterprise repo. How the MRG importer will be dealing with this remains to be specified.
:::

We will use:

- `{other-scopetag}` = the `scopetag`-field in the `scope` section of `{import-saf}`;
- `{other-glossarydir}` = the `glossarydir`-field in the `scope` section of `{import-saf}`;

The [`versions` section](/docs/specs/files/saf#versions) in `{import-saf}` specifies which [terminologies](@) are actively maintained within the other [scope](@), and hence have to be imported. Every such [terminology](@) is specified by an entry in this section, and must hence be processed to import the associated [MRGs](@). 

To specify one such process, we will use:

- `{other-vsntag}` = `vsntag`-field in the element of the `versions` section of `{import-saf}`
- `{other-altvsntags}` = `altvsntags`-field in an element of the `versions` section of `{import-saf}`
- `{other-defaultvsn}` = `defaultvsn`-field in the `scope` section of `{import-saf}`

To import the associated [MRGs](@), here is what we do:

- read the file `{import-scopedir}/{import-glossarydir}/mrg.{other-scopetag}.{other-vsntag}.yaml`, which is the file that contains the [MRG](@) that needs to be imported. If that file doesn't exist, this results in the behavior as specified by the `<action>` value of the `onNotExist` parameter. Default is `throw`.
:::info Editor's note
It may well be possible that contents of the mrg may need to be processed.
This is due to the fact that `scopetag`s are 'local' names for referring to scopes and every scope gets to decide on its own scopetag names. So the meaning of 'scopetags' cannot be transferred across scopes. We have to use universal/fixed identifiers, such as a `scopedir`-URL to identify scopes. Still, local names can be useful (and are necessary), so we'll have to figure out what the impact of this is.
:::

- write the contents to `{my-scopedir}/{my-glossarydir}/mrg.{import-scopetag}.{other-vsntag}.yaml`, overwriting a file that has the same name if that were to exist.
- for every [versiontag](@) in `{other-altvsntags}` (which we call `{other-altvsntag}`), a symbolic link `mrg.{import-scopetag}.{other-altvsntag}.yaml` is created in the `{my-scopedir}/{my-glossarydir}/` directory, that links to the `mrg.{import-scopetag}.{other-vsntag}.yaml` file that was just created in that same directory.

:::info Editor's note
check the paragraph below to see if it is correct, because MRG files are no longer symbolic links, but copies of the MRGs that such symbolic links used to refer to.
:::

After all [MRGs](@) are imported a symbolic link `{my-scopedir}/{my-glossarydir}/mrg.{import-scopetag}.yaml` is created (or overwritten if it already exists), which points to the file `mrg.{import-scopetag}.{other-defaultvsn}.yaml`. 

:::note NOTE the change of the `scopetag` part in the filename!
The name of the [MRG](@) in the [scope](@) from which it is imported may differ from the name of the [MRG](@) that is imported. The reason for this is that the names ([scopetags](@) that are used in these [scopes](@) to refer to the [scope](@) from where [MRGs](@) are imported, may differ.
:::

The [MRG importer](@) logs every error- and/or warning condition that it comes across while processing its configuration file, command-line parameters, and input files, in a way that helps tool-operators and document [authors](@) to identify and fix such conditions.

## Deploying the Tool

The [MRG importer](@) comes with documentation that enables developers to ascertain its correct functioning (e.g. by using a test set of files, test scripts that exercise its parameters, etc.), and also enables them to deploy the tool in a git repo and author/modify CI-pipes to use that deployment.
