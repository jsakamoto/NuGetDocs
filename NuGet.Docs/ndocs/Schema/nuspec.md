# .nuspec Reference

A `.nuspec` file is an XML manifest that contains package metadata. This is used both to build the package and to provide information to consumers. The manifest is always included in a package.

In this topic:

- [General form and schema](#general-form-and-schema)
- [Replacement tokens](#replacement-tokens) (when used with a Visual Studio project)
- [Dependencies](#dependencies)
- [Explicit assembly references](#explicit-assembly-references)
- [Framework assembly references](#framework-assembly-references)
- [Including assembly files](#including-assembly-files)
- [Including content files](#including-content-files)
- [Examples](#examples)

## General form and schema

The current `nuspec.xsd` schema file can be found in the [NuGet GitHub repository](https://github.com/NuGet/NuGet.Client/blob/dev/src/NuGet.Core/NuGet.Packaging/compiler/resources/nuspec.xsd).

<div class="block-callout-info">
	<strong>Note</strong><br>
	On nuget.org you can find a NuGet.Manifest.Schema 2.0.4 package. However, the schema files contained within it are older versions applicable to NuGet 2.0 and earlier. The most up-to-date schema file is best obtained from the GitHub repository.	
</div>

Within this schema, a `.nuspec` file has the following general form:  

	<?xml version="1.0" encoding="utf-8"?>
	<package xmlns="http://schemas.microsoft.com/packaging/2010/07/nuspec.xsd">
	  <metadata>
	    <!-- Required elements-->
	    <id></id>
		<version></version>
		<description></description>
		<authors></authors>

		<!-- Optional elements -->	
		<!-- ... -->    
	  </metadata>
      <!-- Optional 'files' node -->
	</package>
	
For a clear visual representation of the schema, open the schema file in Visual Studio in Design mode and click on the **XML Schema Explorer** link, or open the file as code, right-click in the editor, and select **Show XML Schema Explorer**. Either way you'll get a view like the one below (when mostly expanded):

![Visual Studio Schema Explorer with nuspec.xsd open](/images/docs/SchemaExplorer.png)


### Required metadata elements 

Although the following elements are the minimum requirements for a package, you should consider adding the [optional metadata elements](#optional-metadata-elements) to improve the overall experience developers have with your package.

These elements must appear within a &lt;metadata&gt; element.

<table class="reference">
    <tbody>
        <tr>
            <th>Element</th>
			<th>Description</th>
        </tr>
        <tr>
            <td><strong>id</strong></td>
            <td>
                <p>
                The case-insensitive package identifier, which must be unique across nuget.org or whatever gallery the package will reside in. IDs may not contain spaces or characters that are not valid for a URL, and generally follow .NET namespace rules. See <a href="/ndocs/create-packages/creating-a-package#choosing-a-unique-package-identifier-and-setting-the-version-number">Choosing a unique package identifier</a> for guidance.</p>				
            </td>
        </tr>
        <tr>
            <td><strong>version</strong></td>
            <td>The version of the package, following the <em>major.minor.patch</em> pattern. Version numbers may include a pre-release suffix as described in <a href="/ndocs/create-packages/prerelease-packages#semantic-versioning">Prerelease Packages</a></td>
        </tr>
        <tr>
            <td><strong>description</strong></td>
            <td>
                A long description of the package for UI display.
            </td>
        </tr>
        <tr>
            <td><strong>authors</strong></td>
            <td>A comma-separated list of packages authors, matching the profile names on nuget.org. These are displayed in the NuGet Gallery on nuget.org and are used to cross-reference packages by the same authors.</td>
        </tr>
    </tbody>
</table>

### Optional metadata elements

These elements must appear within a &lt;metadata&gt; element.

#### Single elements

<table class="reference">
    <tbody>
        <tr>
            <th>Element</th>
			<th>Description</th>
        </tr>
        <tr>
            <td><strong>title</strong></td>
            <td>
                A human-friendly title of the package, typically used in UI displays as on nuget.org and the Package Manager in Visual Studio. If not specified, the package ID is used instead.
            </td>
        </tr>
        <tr>
            <td><strong>owners</strong></td>
            <td>
                A comma-separated list of the package creators using profile names on nuget.org. This is often the same list as in authors, and is ignored when uploading the package to nuget.org. See <a href="/ndocs/create-packages/publish-a-package#managing-package-owners-on-nuget-org">Managing package owners on nuget.org</a>                  
            </td>
        </tr>
        <tr>
            <td><strong>projectUrl</strong></td>
            <td>A URL for the package's home page, often shown in UI displays as well as nuget.org.</td>
        </tr>
        <tr>
            <td><strong>licenseUrl</strong></td>
            <td>A URL for the package's license, often shown in UI displays as well as nuget.org.</td>            
        </tr>
        <tr>
            <td><strong>iconUrl</strong></td>
            <td>A URL for a 64x64 image with transparenty background to use as the icon for the package in UI display.
        </tr>
        <tr>
            <td><strong>requireLicenseAcceptance</strong></td>
            <td>A Boolean value specifying whether the client must prompt the consumer to accept the package license before installing the package.</td>
        </tr>
        <tr>
            <td><strong>developmentDependency</strong></td>
            <td><em>(2.8+)</em>  A Boolean value specifying whether the package will be marked as a development-only-dependency, which prevents the package from being included as a dependency in other packages.</td>
        </tr>
        <tr>
            <td><strong>summary</strong></td>
            <td>
                A short description of the package for UI display. If omitted, a truncated version of the <strong>description</strong> is used.
            </td>
        </tr>
        <tr>
            <td><strong>releaseNotes</strong></td>
            <td>
                <em>(1.5+)</em> A description of the changes made in this release of the package, often used in UI like the Updates tab of the Visual Studio Package Manager in place of the package description.
            </td>
        </tr>
        <tr>
            <td><strong>copyright</strong></td>
            <td><em>(1.5+)</em> Copyright details for the package.</td>
        </tr>
        <tr>
            <td><strong>language</strong></td>
            <td>The locale ID for the package. See [Creating localized packages](/ndocs/create-packages/creating-localized-packages).</td>
        </tr>
        <tr>
            <td><strong>tags</strong></td>
            <td>A space-delimited list of tags and keywords that describe the package and aid discoverability of packages through search and filtering mechanisms.</td>
        </tr>
        <tr>
            <td><strong>servicable</strong></td>
            <td><em>(3.3+)</em>For internal NuGet use only</td>
        </tr>
        <tr>
            <td><strong>minClientVersion</strong></td>
            <td><em>(2.5+)</em>  Specifies the minimum version of the NuGet client that can install this package, enfoced by nuget.exe and the Visual Studio Package Manager.</td>
        </tr>
</table>

#### Collection elements

<table>
        <tr>
            <td><strong>packageTypes</strong></td>
            <td><em>(3.3+)</em> A collection of zero or more &lt;packageType&gt; elements specifying the type of the package if other than a traditional dependency package. Each packageType has attributes of <em>name</em> and <em>verion</em>. See <a href="../create-packages/creating-a-package#setting-a-package-type">Setting a package type</a>.
            </td>
        </tr>
        <tr>
            <td><strong>dependencies</strong></td>
            <td>A collection of zero or more &lt;dependency&gt; elements specifying the dependencies for the package. Each dependency has attributes of <em>id</em>, <em>version</em>, <em>include</em> (3.x+), and <em>exclude</em> (3.x+). See <a href="#dependencies">Dependencies</a> below.</td>
        </tr>
        <tr>
            <td><strong>frameworkAssemblies</strong></td>
            <td><em>(1.2+)</em> A collection of zero or more &lt;frameworkAssembly&gt; elements identifying .NET Framework assembly references that this package requires, which ensures that references are added to projects consuming the package. Each frameworkAssembly has <em>assemblyName</em> and <em>targetFramework</em> attributes. See <a href="#specifying-framework-assembly-references-gac">Specifying framework assembly references GAC</a> below.</td>
        </tr>
        <tr>
            <td><strong>references</strong></td>
            <td><em>(1.5+)</em> A collection of zero or more &lt;reference&gt; elements naming assemblies in the package's <code>lib</code> folder that are added as project references. Each reference has a <em>file</em> attribute. &lt;references&gt; can also contain a &lt;group&lt; element with a <em>targetFramework</em> attribute, that then contains &lt;reference&gt; elements. If omitted, all references in <code>lib</code> are included. See <a href="#specifying-explicit-assembly-references">Specifying explicit assembly references.</a> below.  
			</td>
        </tr>
        <tr>
            <td><strong>contentFiles</strong></td>
            <td><em>(3.3+)</em> A collection of &lt;files&gt; elements that identify content files that should be include in the consuming project. These files are specified with a set of attributes that describe how they should be used within the project system. See <a href="#specifying-files-to-include-in-the-package">Specifying files to include in the package</a> below.</td>
        </tr>


    </tbody>
</table>

### Files element

The &lt;package&gt; node may also contain a &lt;files&gt; and/or &lt;contentFiles&gt; nodes as siblings to &lt;metadata&gt; to specify whcih assembly and content files to include in the package. See <a href="#including-assembly-files">Including assembly files</a> and <a href="#including-content-files">Including content files</a> later in this topic for details.</td>
        

## Replacement tokens

When creating a package, the [`nuget pack` command](/ndocs/tools/nuget.exe-cli-reference#pack) will replace $-delimited tokens in the `.nuspec` file's &lt;metadata&gt; node with values that come from either a project file or the `pack` command's `-properties` switch.

On the command line, you specify token values with `nuget pack -properties <name>=<value>;<name>=<value>`. For example, you can use a token such as `$owners$` and `$desc$` in the `.nuspec` and provide the values at packing time as follows:

	nuget pack MyProject.csproj -properties
        owners=janedoe,harikm,kimo,xiaop;desc="Awesome app logger utility"

To use values from a project, specify the tokens described in the table below (AssemblyInfo refers to the file in `Properties` such as `AssemblyInfo.cs` or `AssemblyInfo.vb`).

To use these tokens, you must run `nuget pack` with the project file rather than just the `.nuspec`. For example, when using the following command, the `$id$` and `$version$` tokens in a `.nuspec` file will be replaced with the project's `AssemblyName` and `AssemblyVersion` values:

	nuget pack MyProject.csproj

Typically, when you have a project you'll create the `.nuspec` initially using `nuget spec MyProject.csproj` which will automatically include some of these standard tokens. Note, however, that if a project lacks values for required `.nuspec` elements, then `nuget pack` will fail. Furthermore, if you change project values, be sure to rebuild before creating the package; this can be done conveniently with the pack command's `build` switch. 

With the exception of `$configuration$`, values in the project will be used in preference to any assigned to the same token on the command line.

<table class="reference">
    <tr>
		<th>Token</th>
	    <th>Value source</th>
		<th>Value</th></tr>
    <tr>
        <td>$id$</td>
	    <td>Project file</td>
        <td>AssemblyName from the project file</td>
    </tr>
    <tr>
        <td>$version$</td>
		<td>AssemblyInfo</td>
        <td>AssemblyInformationalVersion if present, otherwise AssemblyVersion</td>
    </tr>
    <tr>
        <td>$author$</td>
		<td>AssemblyInfo</td>
        <td>AssemblyCompany</td>
    </tr>
    <tr>
        <td>$description$</td>
		<td>AssemblyInfo</td>
        <td>AssemblyDescription</td> 
    </tr>
    <tr>
        <td>$copyright$</td>
		<td>AssemblyInfo</td>
        <td>AssemblyCopyright</td> 
    </tr>
    <tr>
        <td>$configuration$</td>
		<td>Assembly DLL</td>
        <td>Configuration used to build the assembly, defaulting to Debug. Note that to create a package using a Release configuration, you always use <code>-properties Configuration=Release</code> on the command line.</td> 
    </tr>
</table>

Tokens can also be used to resolve paths when you include [assembly files](#including-assembly-files) and [content files](#including-content-files). The tokens have the same names as the MSBuild properties, making it possible to select files to be included depending on the current build configuration. For example, if you use the following tokens in the `.nuspec` file:

	<files>
	    <file src="bin\$configuration$\$id$.pdb" target="lib\net40\" />
	</files>

And you build an assembly whose `AssemblyName` is `LoggingLibrary` with the `Release` configuration in MSBuild, the resulting lines in the `.nuspec` file in the package will be as follows:
	
	<files>
	    <file src="bin\Release\library.pdb" target="lib\net40" />
	</files>


## Dependencies

The &lt;dependencies&gt; element within &lt;metadata&gt; contains any number of &lt;dependency&gt; elements that identify other packages upon which the top-level package depends. The attributes for each &lt;dependency&gt; are as follows: 
  
<table class="reference">
    <tr><th>Attribute</th><th>Description</th></tr>
    <tr>
        <td><code>id</code></td>
        <td>(Required) The package ID of the dependency. </td>
    </tr>
    <tr>
        <td><code>version</code></td>
        <td>(Required) The range of versions acceptable as a dependency. See <a href="dependency-versions#version-ranges-in--nuspec-files">Dependency versions</a> for exact syntax.</td>
    </tr>
</table>

For example, the following lines indicate dependencies on `PackageA` version 1.1.0 or higher, and `PackageB` version 1.x. 

	<dependencies>
	  <dependency id="PackageA" version="1.1.0" />
	  <dependency id="PackageB" version="[1,2)" />
	</dependencies>

When creating a `.nuspec` from a project using `nuget spec`, dependencies that exist in that project will be automatically included in the resulting `.nuspec` file.

### Dependency groups 

*Version 2.0+*

As an alternative to a single flat list, dependencies can also be specified according to the framework profile of the target project using &lt;group&gt; elements within &lt;dependencies&gt;.  

Each group has an attribute named `targetFramework` and contains zero or more &lt;dependency&gt; elements. Those dependencies will be installed together when  the target framework is compatible with the project's framework profile.

The &lt;group&gt; element without a `targetFramework` attribute is used as the default or fallback list of dependencies. See [Target frameworks](target-frameworks) for the exact framework identifiers.

<div class="block-callout-info">
	<strong>Note</strong>	
	The group format cannot be intermixed with a flat list.
</div>

The following example shows different variations of the &lt;group&gt; element:

	<dependencies> 
	   <group>
	      <dependency id="RouteMagic" version="1.1.0" />
	   </group>

	   <group targetFramework="net40">
	      <dependency id="jQuery" />
	      <dependency id="WebActivator" />
	   </group>
 
	   <group targetFramework="sl30">
	   </group>
	</dependencies>


## Explicit assembly references
<a name="specifying-explicit-assembly-references"></a>

The &lt;references&gt; element explicitly specifies the assemblies that the target project should reference when using the package. When this element is present, NuGet will add references to only the listed assemblies; it will not add references for any other assemblies in the package's `lib` folder. 

For example, the following &lt;references&gt; element instructs NuGet to add references to only `xunit.dll` and `xunit.extensions.dll` even if there are additional assemblies in the package:

	<references>
	  <reference file="xunit.dll" />
	  <reference file="xunit.extensions.dll" />
	</references>

Explicit reference are typically used for design-time only assemblies. When using [Code Contracts](https://msdn.microsoft.com/library/dd264808.aspx), for example, contract assemblies need to be next to the runtime assemblies that they augment so that Visual Studio can find them, but the contract assemblies need not be referenced by the project or copied into the project's `bin` folder.

Similarly, explicit references can be used for unit test frameworks, such as XUnit, which needs its tools assemblies located next to the runtime assemblies, but does not need them included as project references.

### Reference groups

*Version 2.5+*

As an alternative to a single flat list, references can also be specified according to the framework profile of the target project using &lt;group&gt; elements within &lt;references&gt;. 

Each group has an attribute named `targetFramework` and contains zero or more &lt;reference&gt; elements. Those references will be added to a project when the target framework is compatible with the project's framework profile.

The &lt;group&gt; element without a `targetFramework` attribute is used as the default or fallback list of references. See [Target frameworks](target-frameworks) for the exact framework identifiers.

<div class="block-callout-info">
	<strong>Note</strong>	
	The group format cannot be intermixed with a flat list.
</div>

The following example shows different variations of the &lt;group&gt; element:

	<references> 
	  <group>
	    <reference file="a.dll" />
	  </group>

	  <group targetFramework="net45"> 
	      <reference file="b45.dll" />
	  </group>
 
	  <group targetFramework="netcore45"> 
	    <reference file="bcore45.dll" /> 
	  </group>
	</references>


## Framework assembly references
<a name="specifying-framework-assembly-references-gac"></a>

Framework assemblies are those that are part of the .NET framework and should already be in the global assembly cache (GAC) for any given machine. By identifying those assemblies within the &lt;frameworkAssemblies&gt; element, a package can ensure that required references are added to a project in the event that the project doesn't have such references already. Such assemblies, of course, are not included in a package directly.

The &lt;frameworkAssemblies&gt; element contains zero or more &lt;frameworkAssembly&gt; elements, each of which specifies the following attributes:

<table class="reference">
    <tr>
        <th>Attribute</th>
        <th>Description</th>
    </tr>
    <tr>
        <td>assemblyName</td>
        <td>
            (Required) The fully qualified assembly name.
        </td>
    </tr>
    <tr>
        <td>targetFramework</td>
        <td>
           (Optional) Specifies the target framework to which this reference applies. If omitted, indicates that the reference applies to all frameworks. See <a href="/ndocs/schema/target-frameworks">Target frameworks</a> for the exact framework identifiers.             
        </td>
    </tr>
</table>

The following example shows a reference to `System.Net` for all target frameworks, and a reference to `System.ServiceModel` for .NET Framework 4.0 only:

	<frameworkAssemblies>
	  <frameworkAssembly assemblyName="System.Net"  />

	  <frameworkAssembly assemblyName="System.ServiceModel" 	targetFramework="net40" />	  
	</frameworkAssemblies>



## Including assembly files
<a name="specifying-files-to-include-in-the-package"></a>

If you follow the conventions described in [Creating a Package](/ndocs/create-packages/creating-a-package), you do not have to explicitly specify a list of files in the `.nuspec` file. The `nuget pack` command will automatically pick up the necessary files.

<div class="block-callout-info">
	<strong>Note</strong><br>	
	When a package is installed into a project, NuGet automatically adds assembly references to the package's DLLs, <em>excluding</em> those that are named <code>.resources.dll</code> because they are assumed to be localized satellite assemblies. For this reason, avoid using ".resources.dll" for files that otherwise contain essential package code.  
</div>

To bypass this automatic behavior and explicitly control which files are included in a package, place a &lt;files&gt; element as a child of &lt;package&gt; (and a sibling of &lt;metadata&gt;), identifying each file with a separate &lt;file&gt; element. For example:     

	<files>
	  <file src="bin\Debug\*.dll" target="lib" /> 
	  <file src="bin\Debug\*.pdb" target="lib" /> 
	  <file src="tools\**\*.*" exclude="**\*.log" />
	</files>
  

With NuGet 2.x and earlier, and projects using `packages.config`, the &lt;files&gt; element is also used to include immutable content files when a package is installed. With NuGet 3.3+ and projects using `project.json`, the &lt;contentFiles&gt; element is used instead. See [Including content files](#including-content-files) below for details. 

### File element attributes

Each &lt;file&gt; element specifies the following attributes:

<table class="reference">
    <tr>
        <th>Attriute</th>
        <th>Description</th>
    </tr>
    <tr>
        <td><strong>src</strong></td>
        <td>
            The location of the file or files to include, subject to exclusions specified by the <code>exclude</code> attribute. The path is relative to the <code>.nuspec</code> file unless an absolute path is specified. The wildcard character * is allowed, and the double 
            wildcard ** implies a recursive folder search.
        </td>
    </tr>
    <tr>
        <td><strong>target</strong></td>
        <td>
            The relative path to the folder within the package 
            where the source files will be placed, which must begin with <code>lib</code>, <code>content</code>, or <code>tools</code>.
        </td>
    </tr>
    <tr>
        <td><strong>exclude</strong></td>
        <td>
            A semicolon-delimited list of files or file patterns to exclude from the <code>src</code> location. The wildcard character * is allowed, and the double wildcard ** implies a recursive folder search.
        </td>
    </tr>
</table> 

### Examples

**Single assembly**

	Source file: 
	    library.dll
	
	.nuspec entry:	
		<file src="library.dll" target="lib" />
		
	Packaged result:	 
	    lib\library.dll

**Single assembly specific to a target framework**

	Source file:
		library.dll

	.nuspec entry:
		<file src="assemblies\net40\library.dll" target="lib\net40" />

	Packaged result:
		lib\net40\library.dll

**Set of DLLs using a wildcard**

	Source files: 
		bin\release\libraryA.dll
		bin\release\libraryB.dll

	.nuspec entry: 
		<file src="bin\release\*.dll" target="lib" />

	Packaged result:
		lib\libraryA.dll
		lib\libraryB.dll

**DLLs for different frameworks**

	Source files:
		lib\net40\library.dll
		lib\net20\library.dll

	.nuspec entry (using ** recursive search):
		<file src="lib\**" target="lib" />

	Packaged result: 
		lib\net40\library.dll 
		lib\net20\library.dll

**Excluding files**

	Source files:
		\tools\*.bak
		\tools\*.log
		\tools\build\*.log
 
	.nuspec entries:
	    <file src="tools\*.*" target="tools" exclude="tools\*.bak" />
	    <file src="tools\**\*.*" target="tools" exclude="**\*.log" />
	
	Package result:
		(no files)

## Including content files

Content files are files that a package needs to include in a project, but are considered immutable, that is, they are not intended to be modified by the consuming project. Example content files include:

- Images that are embedded as resources
- Source files that are already compiled
- Scripts that need to be included with the build output of the project
- Configuration files for the package that need to be included in the project but don't need any project-specific changes. 

Content files are included in a package using the &lt;files&lt; element, specifying the `content` folder in the `target` attribute. However, such files are ignored when the package is installed in a project using the `project.json` system in NuGet 3.3+, which instead uses the &;lt;contentFiles&gt; element.

For maximum compatibility with consuming projects, a package ideally specifies content files in both locations. 

### Using the files element for content files 

For content files, simply use the same format as for assembly files, but specify `content` as the base folder in the `target` attribute as shown in the following examples.

**Basic content files**

	Source files: 
		css\mobile\style1.css
		css\mobile\style2.css

	.nuspec entry:
		<file src="css\mobile\*.css" target="content\css\mobile" />

	Packaged result: 
		content\css\mobile\style1.css
		content\css\mobile\style2.css

**Content files with directory structure**

	Source files:
		css\mobile\style.css
		css\mobile\wp7\style.css
		css\browser\style.css

	.nuspec entry: 
		<file src="css\**\*.css" target="content\css" />

	Packaged result:
		content\css\mobile\style.css 
		content\css\mobile\wp7\style.css
		content\css\browser\style.css

**Content file specific to a target framework**

	Source file:
		css\cool\style.css

	.nuspec entry
		<file src="css\cool\style.css" target="Content" />

	Packaged result:
		content\style.css

** Content file copied to a folder with dot in name**

In this case, NuGet sees that the extension in `target` does not match the extension in `src` and thus treats that part of the name in `target` as a folder:

	Source file:
		images\picture.png

	.nuspec entry:
		<file src="images\picture.png" target="Content\images\package.icons" />

	Packaged result:
		content\images\package.icons\picture.png

**Content files without extensions**

To include files without an extension, use the * or ** wildcards:

	Source file:
		flags\installed

	.nuspec entry:
		<file src="flags\**" target="flags" />

	Packaged result:
		flags\installed

**Content files with deep path and deep target**

In this case, because the file extensions of the source and target match, NuGet assumes that the target is a file name and not a folder:

	Source file:
		css\cool\style.css

	.nuspec entry:
		<file src="css\cool\style.css" target="Content\css\cool" />
	    or:
		<file src="css\cool\style.css" target="Content\css\cool\style.css" />

	Packaged result:
		content\css\cool\style.css

**Renaming a content file in the package**

	Source file:
		ie\css\style.css

	.nuspec entry:
		<file src="ie\css\style.css" target="Content\css\ie.css" />

	Packaged result:
		content\css\ie.css

**Excluding files**

	Source file:
		docs\*.txt (multiple files)

	.nuspec entry:
		<file src="docs\*.txt" target="content\docs" exclude="docs\admin.txt" />
		or
		<file src="*.txt" target="content\docs" exclude="admin.txt;log.txt" />
	
	Packaged result:
		All .txt files from docs except admin.txt (first example)
		All .txt files from docs except admin.txt and log.txt (second example)


### Using the contentFiles element for content files

*Verion 3.3+ with project.json only*

By default, a package places content in a `contentFiles` folder (see below) and `nuget pack` will include all files in that folder using default attributes. In this case it's not necessary to include a `contentFiles` node in the `.nuspec` at all. 

To control which files are included, the &lt;contentFiles&gt; element specifies is a collection of &lt;files&gt; elements that identify the exact files include. 
These files are specified with a set of attributes that describe how they should be used within the project system:

<table class="reference">
	<tr>
		<th>Attribute</th>
		<th>Description</th>
	</tr>
	<tr>
		<td>include</td>
		<td>
			(Required) The location of the file or files to include, subject to exclusions specified by the <code>exclude</code> attribute. The path is relative to the <code>.nuspec</code> file unless an absolute path is specified. The wildcard character * is allowed, and the double wildcard ** implies a recursive folder search.
		</td>    		
	</tr>
	<tr>
		<td>exclude</td>
        <td>
            A semicolon-delimited list of files or file patterns to exclude from the <code>src</code> location. The wildcard character * is allowed, and the double wildcard ** implies a recursive folder search.
        </td>
	</tr>
	<tr>
		<td>buildAction</td>
		<td>
			The build action to assign to the contnet item for MSBuild, such as <code>Content</code>, <code>None</code>, <code>Embedded Resource</code>, <code>Compile</code>, etc. The default is <code>Compile</code>. 
		</td>
	</tr>
	<tr>
		<td>copyToOutput</td>
		<td>
			A Boolean indicating whether to copy content items to the build output folder. The default is false.
		</td>
	</tr>
	<tr>
		<td>flatten</td>
		<td>
			A Boolean indicating whether to copy content items to a single folder in the build output (true), or to preserve the folder structure in the package (false). The default is false.
		</td>
	</tr>
</table>

When installing a package, NuGet applies the child elements of &lt;contentFiles&gt; from top to bottom. If multiple entries match the same file then all entries will be applied. The top-most entry will override the lower entries if there is a conflict for the same attribute.

#### Package folder structure

The package project should structure content using the following pattern:

	/contentFiles/{codeLanguage}/{TxM}/{any?}

- `codeLanguages` may be `cs`, `vb`, `fs`, `any`, or the lowercase equivalent of a given `$(ProjectLanguage)`
* `TxM` is any legal target framework moniker that NuGet supports (see [Target frameworks](/ndocs/schema/target-frameworks)).
* Any folder structure may be appended to the end of this syntax.

For example:

	Language- and framework-agnostic:
		/contentFiles/any/any/config.xml

	net45 content for all lanugages
		/contentFiles/any/net45/config.xml

	C#-specific content for net45 and up
		/contentFiles/cs/net45/sample.cs

Empty folders can use `.` to opt out of providing content for certain combinations of language and TxM, for example:

	/contentFiles/vb/any/code.vb
	/contentFiles/cs/any/.
	

#### Example contentFiles section

	<contentFiles>
	    <!-- Embed image resources -->
	    <files include="any/any/images/dnf.png" buildAction="EmbeddedResource" />
	    <files include="any/any/images/ui.png" buildAction="EmbeddedResource" />

	    <!-- Embed all image resources under contentFiles/cs/ -->
	    <files include="cs/**/*.png" buildAction="EmbeddedResource" />

	    <!-- Copy config.xml to the root of the output folder -->
	    <files include="cs/uap/config/config.xml" buildAction="None" copyToOutput="true" flatten="true" />

	    <!-- Copy run.cmd to the output folder and keep the directory structure -->
		<files include="cs/commands/run.cmd" buildAction="None" copyToOutput="true" flatten="false" />

	    <!-- Include everything in the scripts folder except exe files -->
	    <files include="cs/net45/scripts/*" exclude="**/*.exe"  buildAction="None" copyToOutput="true" />	   
	</contentFiles>


## Example .nuspec files

**A simple .nuspec that does not specify dependencies or files**

	<?xml version="1.0" encoding="utf-8"?>
	<package xmlns="http://schemas.microsoft.com/packaging/2010/07/nuspec.xsd">
	  <metadata>
	    <id>sample</id>
	    <version>1.2.3</version>
	    <authors>Kim Abercrombie, Franck Halmaert</authors>
	    <description>Sample exists only to show a sample .nuspec file.</description>
	    <language>en-US</language>
	    <projectUrl>http://xunit.codeplex.com/</projectUrl>
	    <licenseUrl>http://xunit.codeplex.com/license</licenseUrl>
	  </metadata>
	</package>

**A .nuspec with dependencies**

	<?xml version="1.0" encoding="utf-8"?>
	<package xmlns="http://schemas.microsoft.com/packaging/2010/07/nuspec.xsd">
	  <metadata>
	    <id>sample</id>
	    <version>1.0.0</version>
	    <authors>Microsoft</authors>
	    <dependencies>
	      <dependency id="another-package" version="3.0.0" />
	      <dependency id="yet-another-package"/>
	    </dependencies> 
	  </metadata>
	</package>

**A .nuspec with files**

	<?xml version="1.0"?>
	<package xmlns="http://schemas.microsoft.com/packaging/2010/07/nuspec.xsd">
	  <metadata>
	    <id>routedebugger</id>
	    <version>1.0.0</version>
	    <authors>Jay Hamlin</authors>
	    <requireLicenseAcceptance>false</requireLicenseAcceptance>
	    <description>Route Debugger is a little utility I wrote...</description>
	  </metadata>
	  <files>
	    <file src="bin\Debug\*.dll" target="lib" />
	  </files>
	</package>

**A .nuspec with framework assemblies**

	<?xml version="1.0"?>
	<package xmlns="http://schemas.microsoft.com/packaging/2010/07/nuspec.xsd">
	  <metadata>
	    <id>PackageWithGacReferences</id>
	    <version>1.0</version>
	    <authors>Author here</authors>
	    <requireLicenseAcceptance>false</requireLicenseAcceptance>
	    <description>
	        A package that has framework assemblyReferences depending 
	        on the target framework.
	    </description>
	    <frameworkAssemblies>
	      <frameworkAssembly assemblyName="System.Web" targetFramework="net40" />
	      <frameworkAssembly assemblyName="System.Net" targetFramework="net40-client, net40" />
	      <frameworkAssembly assemblyName="Microsoft.Devices.Sensors" targetFramework="sl4-wp" />
	      <frameworkAssembly assemblyName="System.Json" targetFramework="sl3" />
	    </frameworkAssemblies>
	  </metadata>
	</package>

In this example, the following will be installed for specific project targets:

* .NET4 -> `System.Web`, `System.Net`
* .NET4 Client Profile -> `System.Net`
* Silverlight 3 -> `System.Json`
* Silverlight 4 -> `System.Windows.Controls.DomainServices` 
* WindowsPhone -> `Microsoft.Devices.Sensors`
