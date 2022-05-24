# docassemble.ClinicIntakeFax

testing link: 
https://apps-test.suffolklitlab.org/run/ClinicFax



## Authors

* cl.asouza@suffolk.edu
* cl.ktaraschke@suffolk.edu
* mcarroll11@su.suffolk.edu

Quick explanation of what this app does:  https://www.youtube.com/watch?v=jog3gxZc090&t=208s
The app currently allows users to fax either the Defender's HIPAA form, the Juv. Defender's form, or BMC Form to any hospital's fax machine.
It does by following these steps:
  1) requires log in
  2) asks basic information about the clinic student
  3) asks information about the client
  4) shows a preiview that can be viewed by the client
  5) allows the client to sign the form
  6) asks the clinic student about the hospital they would like to send the fax to
  7) creates a fax cover sheet based on what the student has input
  8) combines the cover sheet with the signed form
  9) sends the form to the number given by the student
  10) sends a pdf version of the faxed form to the students email


For future students who may be working on this project, here are some more detailed instructions on how this app works.

The code first has a list of objects that the app needs, such as: 
```yaml
- fax: Individual
- supervisor: ALPeopleList.using(target_number=1,ask_number=True)
- court: DAObject
```

The next significant part of the code is the Main order, which tells the interview the order in which to ask questions.
This block is where "show if" functions can control when and if certain questions are asked.  An example is:

```python
  if parent_choice:
    parent_relationship
```
    
After asking all questions, the form then confirms that you want to send the form by check: `hospital[0].got_result`

Then it sends the form with `fax_result_Def`

Then it sends the email pdf to the student with `email_sent_ok_def`

Finally, students are presented with the "Your fax has been sent" screen.

If you would like to add a new form, you will have to do a few things.

First, you will have to make the form usable using the weaver

Then, you will use the usable template to the package and tell the code what each variable is going to be used as in your code.
Here is an example:

```yaml
id: Defenders HIPAA
attachment:
  name: Defenders HIPAA
  variable name: Defenders_HIPAA[i]
  filename: Defenders_HIPAA
  skip undefined: True
  pdf template file: Defenders_Clinic_HIPAA.pdf
  editable: False
  fields:
      - "client_name__1": ${ client[0].name }
      - "student_attorney": ${ studentattorney[0].name }
      - "hospital.name": ${ hospital[1].name }
      - "trial_court": ${ trial_court } 
      - "all_records": ${ all_records }
      - "some_records": ${ some_records } 
      - "records_relating": ${ what_records}
      - "client_signature": ${ client[0].signature }
      - "todays_date__1": ${ today(format='MM/dd/YYYY') }
      - "client_dob": ${ client.birthdate.format('MM/dd/YYY') }
      - "admission_dates": ${ admission_release_dates }
      - "hospital.fax": ${ hospital[1].fax_number }
```

Next, you will have to add your Form as an option to Forms_choice

Then have to add questions that are not currently asked or modify the main order so the questions you need are asked when your form is choosen

Lastly, you will have to duplicate the sections that are regarding the sending of the fax and add a section to the main order.
Here is what the BMC part looks like:

```python
  if Forms_choice["Defenders Generic HIPAA Form"]: 
    trial_court
    records_choice
    all_records
```
    
Once you have done that and modify the exit page, your new form should be succesfully added.
