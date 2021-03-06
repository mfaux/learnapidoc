---
title: "Swagger tutorial"
permalink: /pubapis_swagger.html
course: "Documenting REST APIs"
sidebar: docapis
weight: 8.2
section: restapispecifications 
path1: /restapispecifications.html
---

In this tutorial, you'll learn how to use Swagger UI to parse through a sample Swagger spec file and display an output that look similar to the [Swagger Petstore example](http://petstore.swagger.io/). The sample API will be the same weather API from Mashape [used earlier in this course](docapis_scenario_for_using_weather_api.html). For a more detailed conceptual overview of Swagger, see [Swagger Overview](pubapis_swagger_intro.html).

{% if site.target == "web" %}
* TOC
{:toc}
{% endif %}

## Swagger overview

Swagger is one of the most popular specifications for REST APIs for a number of reasons:

* Swagger generates an interactive API console for people to quickly learn about and try the API.
* Swagger generates the client SDK code needed for implementations on various platforms.
* The Swagger file can be auto-generated from code annotations on a lot of different platforms.
* Swagger has a strong community with helpful contributors.

The Swagger spec provides a way to describe your API using a specific JSON or YAML schema that outlines the names, order, and other details of the API.

You can code this Swagger file by hand in a text editor, or you can auto-generate it from annotations in your source code. Different tools can consume the Swagger spec file to generate the interactive API documentation. In the following tutorial, I'll show you how to code the Swagger spec file by hand.

{: .note}
The interactive API documentation generated by the Swagger shows the resources, parameters, requests, and responses of your API's endpoints. However, you'll still need a user guide that explains the many other aspects of how to use your API, such as how to get an API key, how to use the endpoints together to achieve an end goal, rate limiting, and so on.

## The Swagger Petstore example

To get a better understanding of Swagger, let's explore the Petstore example. Swagger can be rendered into different visual displays based on the visual framework you decide to use to parse the Swagger spec. In the Petstore example, the site is generated using [Swagger UI](https://github.com/swagger-api/swagger-ui).

<a href="http://petstore.swagger.io/"><img src="images/swaggerpetstoreui.png" alt="Petstore UI" /></a>

There are three resource groups: pet, store, and user.

### Create a pet

1. In the **Pet** resource section, click the **POST `/pet`** method to expand it.
2. Click the **Try it out** button on the right. The request body becomes editable.
3. Change the value for the first `id` tag. (Make it unique so that others don't use the same `id`, and don't start with `0`.)
4. Change the `name` value to something unique. Here's an example:

   ```json
   {
   "id": 37987,
   "category": {
    "id": 0,
    "name": "string"
   },
   "name": "Mr. Fluffernutter",
   "photoUrls": [
    "string"
   ],
   "tags": [
    {
      "id": 0,
      "name": "string"
    }
   ],
   "status": "available"
   }
   ```

5. Click **Execute**.

	Look and see the response. It shows the [curl submitted](docapis_make_curl_call.html) and the [response](docapis_doc_sample_responses) from the API.

  {% include important.html content="You've actually just created a pet with this API. You now need to take responsibility for your pet and begin feeding and caring for it! All joking aside, most users don't realize they're playing with real data when they execute responses in an API (using their own API key). This test data may be something you have to wipe clean when you transition from exploring and learning about the API to actually using the API for production use." %}

### Find your pet by the ID

1. In the same **Pet** section, expand the **GET `pet/{petId}`** method.
2. Click **Try it out**.
3. Insert your pet's ID in the `petId` value box.
4. Click **Execute**.

	 The request in curl format is shown, and your response appears below it. The response shows the name of the pet you created.

   By default, the response will be in XML.

5.  Change the **Response content type** selector to **application/json** and click **Execute** again. The pet response is returned in JSON format.

## Sorting out the Swagger components

As explained in the [Swagger overview](pubapis_swagger.html), Swagger has a number of different pieces. Here's a good time for a quick reminder.

* **[Swagger spec](https://github.com/swagger-api/swagger-spec)**: The Swagger spec is the official schema about name and element nesting, order, and so on. If you plan on hand-coding the Swagger files, you'll need to be extremely familiar with the Swagger spec.
* **[Swagger editor](http://editor.swagger.io/#/)**: The Swagger Editor is an online editor that validates your YML-formatted content against the rules of the Swagger spec. YML is a syntax that depends on spaces and nesting. You'll need to be familiar with YML syntax and the rules of the Swagger spec to be successful here. The Swagger editor will flag errors and give you formatting tips. (Note that the Swagger spec file can be in either JSON or YAML format.)
* **[Swagger-UI](https://github.com/swagger-api/swagger-ui)**: The Swagger UI is an HTML/CSS/JS framework that parses a JSON or YML file that follows the Swagger spec and generates a navigable UI of the documentation. This is the tool that transforms your spec into the Swagger Petstore-like UI output.
* **[Swagger-codegen](https://github.com/swagger-api/swagger-codegen)**: This utility generates client SDK code for a lot of different platforms (such as Java, JavaScript, Scala, Python, PHP, Ruby, Scala, and more). This client code helps developers integrate your API on a specific platform and provides for more robust implementations that might include more scaling, threading, and other necessary code. An SDK is supportive tooling that helps developers use the REST API.

## Some sample Swagger implementations

Before we get into this Swagger tutorial with another API (other than Petstore), check out a few Swagger implementations:

* [Reverb](https://reverb.com/swagger#!/accounts)
* [VocaDB](http://vocadb.net/swagger/ui/index)
* [Watson Developer Cloud](http://www.ibm.com/smarterplanet/us/en/ibmwatson/developercloud/apis/)

Most of them look pretty much the same, with minimal branding. You'll notice the documentation is short and sweet in a Swagger implementation. This is because the Swagger display is meant to be an interactive experience where you can try out calls and see responses &mdash; using your own API key to see your own data. It's the learn-by-doing-and-seeing-it approach.

{% include random_ad.html %}

As you explore Swagger, you may notice a few limitations with the approach:

* There's not much room to describe in detail the workings of the endpoint in Swagger. If you have several paragraphs of details and gotchas about a parameter, it's best to link out from the description to another page in your docs.
* The Swagger UI looks mostly the same for each output. You can modify the source files and regenerate the output, but doing so requires more advanced coding skills.
* The Swagger UI will be a separate site from your other documentation. This means in your regular docs, you'll most likely link to Swagger as the reference for your endpoints. You don't want to duplicate your parameter descriptions and other details in two different sites.

## Create a Swagger UI display

In this activity, you'll create a Swagger UI display for the weatherdata endpoint in this [Mashape Weather API](https://www.mashape.com/fyhao/weather-13#weatherdata). (If you're jumping around in the documentation, this is a simple API that we used in earlier parts of the course.) You can see a demo of what we'll build [here](http://idratherassets.com/restapicourse/swagger/):

<a href="http://idratherassets.com/restapicourse/swagger/"><img src="images/myswagger.png" alt="Swagger UI demo" /></a>

### a. Create a Swagger spec file

To create a Swagger spec file:

1.  Go to the [Swagger online editor](http://editor.swagger.io/#/).

2.  Create the Swagger spec here.

    You could just customize this sample YML file shown in the Swagger editor with the weather details. However, if you're new to Swagger it will take you some time to learn the spec. For the sake of convenience, just go to this  file &mdash; <a href="http://idratherassets.com/restapicourse/swagger/swagger_weather.yml">swagger_weather.yaml</a> &mdash; and copy and paste its code into the Swagger editor. The next tutorial, [Swagger spec deep dive](pubapis_swagger_spec_deep_dive.html), will get into the nuts and bolts of the Swagger spec.

    Notice that this spec is written in [YAML](pubapis_yaml.html) instead of [JSON](docapis_analyze_json.html). YAML syntax is a more human-readable form of JSON. With YML, spacing matters. New levels are set with two indented spaces. The colon indicates an object. Hyphens represent a sequence or list (like an array). If you [download this file](http://idratherassets.com/restapicourse/swagger/swagger_weather.yml) instead of copy-and-pasting it above, you're less likely to run into spacing errors.

    The Swagger editor shows you how the file will look in the output. You'll also be able to see if there are any validity errors. Without this online editor, you would only know that the YML syntax is valid/invalid when you run the code (and potentially see errors indicating that the YAML file couldn't be parsed).

    {: .tip}
    This spec follows Swagger 2.0. Swagger 3.0 is almost released. You can convert your 2.0 spec to 3.0 using [API Transformer](https://apimatic.io/transformer). In this initial Swagger tutorial, I won't delve deeper into how to create the [spec](https://github.com/OAI/OpenAPI-Specification), but in the next tutorial, [Swagger spec deep dive](pubapis_swagger_spec_deep_dive.html), I'll explain more about the available properties and approach. You can also see this [tutorial from API Handyman](https://apihandyman.io/writing-openapi-swagger-specification-tutorial-part-1-introduction/).

3.  Make sure the YAML file is valid in the Swagger editor. If there are any errors, fix them.
4.  Go to **File > Download YAML** and save the file as "swagger_weather.yaml" on your computer. You can also choose JSON as the format, but YAML is more readable and works just as well.

    {: .note}
    You could also just copy or download the swagger_weather.yaml code and use it directly, without going through the [Swagger online editor](http://editor.swagger.io/#/). The Editor or Download process doesn't change the content displayed in the Swagger Editor. I just have you go through this exercise to become familiar with the Swagger Editor. Getting your spec valid is tedious and requires a lot of fine-tuning. In this effort, having the real-time validation analysis from Swagger Editor is invaluable.

### b. Set Up the Swagger UI

1. Go to the [Swagger UI project](https://github.com/swagger-api/swagger-ui) Github project.
2. Click **Clone or download**, and then click **Download ZIP** button. Download the files to a convenient location on your computer and extract the files. (Note that you can also do a git clone to get the code as well.)

	 The only folder you'll be working with here is the **dist** folder (short for distribution). Everything else is used only if you're regenerating the files, which is beyond the scope of this tutorial.

3.  Drag the **dist** folder out of the swagger-ui-master folder so that it stands alone. Then delete the swagger-ui-master folder and zip file.
4.  Inside your **dist** folder, open **index.html** in a text editor such as Atom or Sublime Text.
5.  Look for the following code:

    ```js
    url: "http://petstore.swagger.io/v2/swagger.json",
    ```

5.  Change the `url` value from `http://petstore.swagger.io/v2/swagger.json` to the following:

    ```js
    url: "swagger_weather.yml",
    ```

    Save the file.

6.  Drag the **swagger.yaml** file that you created earlier into the same directory as the index.html file you just edited. Your file structure should look as follows:

    ```
    ├── swagger
    │   ├── favicon-16x16.png
    │   ├── favicon-32x32.png
    │   ├── index.html
    │   ├── oauth2-redirect.html
    │   ├── swagger-ui-bundle.js
    │   ├── swagger-ui-bundle.js.map
    │   ├── swagger-ui-standalone-preset.js
    │   ├── swagger-ui-standalone-preset.js.map
    │   ├── swagger-ui.css
    │   ├── swagger-ui.css.map
    │   ├── swagger-ui.js
    │   ├── swagger-ui.js.map
    │   ├── swagger30.yml
    │   └── swagger_weather.yml
    ```

7.  Upload the folder to a web server and go to the folder path. For example, if you called your directory **dist** (leaving it unchanged), you would go to **http:/myserver.com/dist**.


    Note that if you go to **http:/myserver.com/dist/index.html**, Swagger UI seems to get the path to the swagger_weather.yml file wrong, writing it as **http:/myserver.com/dist/index.html/swagger_weather.yml** instead. As a foolproof approach, you can code the absolute URL to the Swagger file (`url: "http://myserver.com/folder/swagger_weather.yaml"`) in the index file rather than the relative URL as you did with (`url: "swagger_weather.yaml"`).

{: .tip}
Here's [a sample Swagger UI folder uploaded](http://idratherassets.com/restapicourse/swagger/).

If you don't have easy access to a web server, you can download [XAMPP](https://www.apachefriends.org/index.html) to run a server locally on your own machine. For detailed instructions, see the [next section](#xampptutorial)

### c. View the Swagger UI Display Locally with XAMPP {#xampptutorial}

1. Download and install [XAMPP](https://www.apachefriends.org/). Choose the version that's right for your machine.
2. After installation, in your **Applications** folder (on a Mac), open the **XAMPP** folder and start the **manager-osx** console. (For Windows, run XAMPP from your list of Applications.)
2. Click the **Manage Servers** tab in the console manager.
3. Select **Apache Web Server** and click **Start**.
4. Click the **Welcome** tab. Then click **Open Application Folder**.
5. Drag the **dist** folder into the **htdocs** folder. (Everything inside htdocs is accessible from the localhost path when the XAMPP server is running.)
6. In your browser, go to **localhost/dist**.

The Swagger UI display should appear.

### Interact with the Swagger UI

1.  Go to the URL where you uploaded your Swagger files. Alternatively, if you're using XAMPP locally, go to **localhost/dist**.
2.  In the upper-right corner, click **Authorize** and enter your [Mashape API key](docapis_get_auth_keys.html) in the dialog box's **value** field. If you don't have a Mashape API key, you can use `EF3g83pKnzmshgoksF83V6JB6QyTp1cGrrdjsnczTkkYgYrp8p`.
3.  Expand one of the endpoints.
4.  Click **Try it out**. The parameters become editable.
5.  In another tab, go to [Google Maps](https://maps.google.com) and search for an address.
6.  Get the latitude and longitude from the URL, and plug it into your Swagger UI. For example, `1.3319164` for `lat`, `103.7231246` for `lng`. This is Singapore.


	  If successful, you should see something in the response body like this:

    ```
	  9c, Mostly Cloudy at South West, Singapore
    ```

    If you see a response that says "Not supported," try the `lat` and `lng` coordinates used here. Try working with each of your endpoints and see the data that gets returned.

	  Note that if you refresh the page, you'll need to re-enter your API key.

## Auto-generating the Swagger file from code annotations

Instead of coding the Swagger file by hand, you can also auto-generate it from annotations in your programming code. There are many Swagger libraries for integrating with different code bases. These Swagger libraries then parse the annotations that developers add and generate the same Swagger file that you produced manually using the earlier steps.

By integrating Swagger into the code, you allow developers to easily write documentation, make sure new features are always documented, and keep the documentation more current. Here's a [tutorial on annotating code with Swagger for Scalatra](http://www.infoq.com/articles/swagger-scalatra). The annotation methods for Swagger doc blocks vary based on the programming language.

For other tools and libraries, see [Swagger services and tools](http://swagger.io/open-source-integrations/).
