##
# LOAD TEST

## Dependencies:

- Node.js

- Artillery.io
  - npm install artillery -g
  - npm install artillery-plugin-expect -g
  - npm install artillery-plugin-fuzzer -g
- Access to Hope Swagger Staging
- VSCode
- Test scripts [**here**](https://drive.google.com/file/d/17CyEDUX3PhVfEIc4RDLWLrQhWbAqtMZm/view?usp=sharing)
- Token Generator [**here**](https://drive.google.com/open?id=1cKWBZSWKFqZbuC-lqEqNc_HvUQihNFnG)


 [**Google latest documentation link here**](https://docs.google.com/document/d/1qxw1UEdX6ibl5Xvx5JLCCErBKsREB5WQvh_-3zX7lKE/edit?usp=sharing)

## Setup access

1. Go to [**Swagger Staging UI**](https://staging.static.tempest.hope.addimedical.netzon.se/api/swagger)
  1. Find and click Service
  2. Click GET and then &#39;Try it out&#39;
2. Download [**this token generator**](https://drive.google.com/open?id=1cKWBZSWKFqZbuC-lqEqNc_HvUQihNFnG) and extract
  1. Click HOPE\_cmd
  2. Type &quot;pro&quot; to list down all providers (By default, this is set to _ **Staging** _)
  3. Type &quot;token [code] SUPPORT01&quot;
    1. Replace [code] with provider code (e.g. &quot;token NETZON SUPPORT01&quot;)
  4. Now, copy the generated code
3. Go back to Swagger Staging UI > Service and click GET.
  1. Click the Lock icon on the right side

![](RackMultipart20200421-4-1ld9wdj_html_ce51fac968ad32c7.png)

  1. In the ServiceAccountAuthorization input field, paste the provider code (e.g. &quot;bearer NETZON&quot;)
  2. Click _ **&#39;Authorize** __&#39;_
  3. Click &#39;_ **Close&#39;** _
1. Paste the generated code in Swagger Staging UI > Service > &quot;_ **userToken** _&quot; input field
  1. Input &quot;en&quot; in &quot;_ **Accept-Language** _&quot; input field
  2. Click &#39;Execute&#39;
2. Scroll down and copy the token (below the &quot;token&quot;)
3. Click the &#39;Lock&#39; icon again, input &quot;bearer&quot; in the &quot;_ **Authorization** _&quot; input field and paste token (e.g. &quot;bearer [token])

## Use Authorization on config scripts

1. Open the [**load test folder**](https://drive.google.com/file/d/17CyEDUX3PhVfEIc4RDLWLrQhWbAqtMZm/view?usp=sharing)
  1. Go to staging > config folder and open &quot;_ **config.yml** _&quot; file
  2. Scroll down to &quot;_ **Authorization** _&quot; and &quot;_ **ServiceAccountAuthorization** _&quot;, replace the &quot;ServiceAccountAuthorization&quot; and &quot;Authorization&quot; variables with the one you used in _Setup Access_ process > **step 3b** and **step 6**
2. Repeat the process on the other config file.
  1. Go to staging > scripts and open **&quot;** _ **postPatient.yml&quot;** _
  2. Scroll down to &quot;_ **Authorization** _&quot; and &quot;_ **ServiceAccountAuthorization** _&quot;, replace the &quot;ServiceAccountAuthorization&quot; and &quot;Authorization&quot; variables with the one you used in _Setup Access_ process > **step 3b** and **step 6**
    1. Note: We need a separate config file for _ **&quot;postPatient.yml&quot;** _ since we only need to run this once.

## Patient creation

1. Open the load test folder
2. Open staging > scripts and open &quot;_ **postPatient** _ **.** _ **yml** _&quot; file with VSCode
3. Scroll down to **&quot;variables:&quot;** section
  1. In &quot; **personnummer:**&quot;, input your desired personnummer of the patient (e.g. &quot;012345567&quot;, don&#39;t remove quotation mark)
  2. In &quot; **firstname**&quot;, input your desired given name of the patient (e.g. &quot;John&quot;, don&#39;t remove quotation mark)
  3. In &quot; **lastname:**&quot;, input your desired family name of the patient (e.g. &quot;Doe&quot;, don&#39;t remove quotation mark)
4. Open staging, double click &quot;_ **postPatient.cmd** _&quot; file
  1. Should not show error while executing
5. Now you&#39;ve created a patient

## RUNNING LOAD TESTS ON DESIRED CONFIGURATION

In the staging > config > config.yml, you can configure on how load test should run

- In the &quot;phases&quot; section,
  - **duration** - refers to the duration the script will run
  - **arrivalRate** - number of new virtual users created executing the script per second
  - **name** - name of the phase (can add multiple phases, e.g. Normal, medium or heavy load)

For example:

phases:

- duration: 10  ##Duration of test will be running

- arrivalRate: 2   ##Arrival of new virtual user per second (average virtual users)

- name: &quot;Normal Load&quot;    ##Name of phase. Can set multiple phases

The configuration above will be running for 10 seconds and an average of 2 virtual users will execute the scripts. Technically, after completely executing this script, there will be a total of 20 requests made.

## Add Care plan to the created patient

1. Go to [**Swagger Staging UI**](https://staging.static.tempest.hope.addimedical.netzon.se/api/swagger)
  1. Find and click &quot;_ **Patient** _&quot; and click &quot;_ **GET /api/patient&quot;** _
  2. Click &#39;Try it out&#39;
  3. In the &#39;given&#39; input field, input the given name entered in Patient creation step **3b**
  4. In the &#39;family&#39; input field, input the given name entered in Patient creation step **3c**
  5. In the &#39;Accept-Language&#39;, input &quot;en&quot;
  6. Click _ **&#39;Execute&quot;** _
2. Scroll down below to &#39;Response body&#39;
  1. Scroll down until you see the _ **&quot;guid&quot;** _, copy the 2nd &quot;guid&quot;, not the first one

![](RackMultipart20200421-4-1ld9wdj_html_c7af8039a8fcc181.png)

1. Go to staging > config folder and open &quot;_ **config.yml** _&quot; file
  1. Paste the guid in &quot;_ **patientGUID** _&quot;
  2. Update the **&quot;careplanName&quot;** to change the care plan&#39;s name. (any)
2. Go to staging, double click &quot;postCareplan.cmd&quot;
  1. Should not show error while executing
3. Now you&#39;ve added a care plan to your created patient

## Add Activities under the created care plan of the user

1. Go to [**Swagger Staging UI**](https://staging.static.tempest.hope.addimedical.netzon.se/api/swagger)
  1. Find and click CarePlan
  2. Click GET /api/careplan/{patientGuid
  3. Click &#39;Try it out&#39;
  4. In the _ **&quot;patientGuid&quot;** _, input the patientGUID from staging > config > config.yml under variables
  5. In _ **&quot;Accept-Language&quot;** _ input field, input &quot;en&quot;
  6. Click &#39;Execute&#39;
2. On the Response body, scroll down until you find the created careplan
  1. Copy the guid

![](RackMultipart20200421-4-1ld9wdj_html_2160537593c04d9e.png)

  1. Paste it in staging > config > config.yml in _ **&quot;careplanGUID&quot;,** _ under variables
1. By default, the Activity that will be created is
  1. A recurring activity type &#39;Weight&#39;, repeats in 10 days. 1
  2. If you want to change the Activity, replace the **&quot;activityCode&quot;** under variables. Activity codes can be found in Swagger UI > Activity Type > GET /api/activitytypes
2. Go to staging > double click _ **&quot;postActivityUnderCareplan.cmd&quot;** _

## Getting calendar data of a patient _(assuming you&#39;ve done the process above)_

1. Go to staging > config > config.yml, change the start date of calendar by changing the value of
  1. &quot;_ **calendarStart&quot;** _ for start date (follow the given format)
  2. &quot; **calendarEnd&quot;** for end date (follow the given format)
2. Go to staging, double click **&quot;getCalendarData.cmd&quot;**