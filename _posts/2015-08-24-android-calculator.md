---
layout: post
title:  "Android Calculator"
date:   2015-08-24 12:09:42
categories: tutorial
---

This tutorial helps you to build your own custom simple Calculator. It is purposefully designed to be very simple and to calculate very simple calculations like additions, subtractions, divisions and multiplications and help you familiarize about the layouts, project structure and coding style.  

For the full project checkout my [GitHub repo](https://github.com/vishnusosale/Calculator). If there are any issues with the code please post the issue in the GitHub repo. Fork it, clone it, share it.

Let's start by creating the UI in the xml file. We shall have 2 EditText to input two numbers.

{% highlight xml %}
<EditText
  android:id="@+id/numAEditText"
  android:layout_width="match_parent"
  android:layout_height="wrap_content"
  android:layout_alignParentEnd="true"
  android:layout_marginTop="20dp"
  android:hint="@string/numAHint"
  android:inputType="numberSigned|numberDecimal" />

<EditText
  android:id="@+id/numBEditText"
  android:layout_width="match_parent"
  android:layout_height="wrap_content"
  android:layout_alignParentEnd="true"
  android:layout_below="@+id/numAEditText"
  android:layout_marginTop="20dp"
  android:hint="@string/numBHint"
  android:inputType="numberSigned|numberDecimal" />
{% endhighlight %}

For simplicity let's create 4 buttons, each to add, subtract, multiply and divide. I have surrounded the buttons inside LinearLayout for a better UI. You can have the buttons placed anywhere on the screen and experiment.  

The LinearLayout is set to horizontal orientation, meaning that all the child Views in the LinearLayout will be placed horizontally. If the LinearLayout is set to vertical orientation then all the child Views will be placed vertically. My implementation of the buttons is shown below.

{% highlight xml %}
<LinearLayout
  android:id="@+id/linearLayout"
  android:layout_width="wrap_content"
  android:layout_height="wrap_content"
  android:layout_below="@+id/numBEditText"
  android:layout_marginTop="20dp"
  android:orientation="horizontal">

        <Button
            android:id="@+id/addButton"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:text="@string/add" />

        <Button
            android:id="@+id/subButton"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:text="@string/sub" />

        <Button
            android:id="@+id/divButton"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:text="@string/div" />

        <Button
            android:id="@+id/mulButton"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:text="@string/mul" />

</LinearLayout>
{% endhighlight %}

We will also include a TextView which displays the answers for each calculations.

{% highlight xml %}

<TextView
    android:id="@+id/answerTextView"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:layout_below="@+id/linearLayout"
    android:layout_centerHorizontal="true"
    android:layout_centerVertical="true"
    android:layout_marginTop="20dp"
    android:textAppearance="?android:attr/textAppearanceLarge"/>

{% endhighlight %}

The resulting UI is shown below.  

![UI_Screenshot](/assets/screenshot0.png)


The only thing left is to wire the UI and write the logic for all the calculations.  

We declare two functions in onCreate() namely, initViews() and handleButtonClickEvents(). The initViews() function lets us wire the Views. The handleButtonClickEvents() function handles all the button clicks.

The handleButtonClickEvents() function is defined as follows.  

{% highlight java %}

private void handleButtonClickEvents() {

        addButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if (isEmptyEditText()) {
                    Toast.makeText(getApplicationContext(), "Enter 2 numbers", Toast.LENGTH_SHORT).show();
                } else {
                    answerTextView.setVisibility(View.VISIBLE);
                    answerTextView.setText("Answer: " + addNumbers());
                }
            }
        });

        subButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if (isEmptyEditText()) {
                    Toast.makeText(getApplicationContext(), "Enter 2 numbers", Toast.LENGTH_SHORT).show();
                } else {
                    answerTextView.setVisibility(View.VISIBLE);
                    answerTextView.setText("Answer: " + subNumbers());
                }
            }
        });

        divButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if (isEmptyEditText()) {
                    Toast.makeText(getApplicationContext(), "Enter 2 numbers", Toast.LENGTH_SHORT).show();
                } else {
                    answerTextView.setVisibility(View.VISIBLE);
                    answerTextView.setText("Answer: " + divNumbers());
                }
            }
        });

        mulButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if (isEmptyEditText()) {
                    Toast.makeText(getApplicationContext(), "Enter 2 numbers", Toast.LENGTH_SHORT).show();
                } else {
                    answerTextView.setVisibility(View.VISIBLE);
                    answerTextView.setText("Answer: " + mulNumbers());
                }
            }
        });
    }

{% endhighlight %}


Whenever the user clicks on any button, we check if the EditText fields are empty. The function isEmptyEditText() checks if either of the EditText are empty. isEmptyEditText() returns true even if one EditText is empty. We Toast a message to the user if the EditTexts are empty. isEmptyEditText() is defined as follows.  

{% highlight java %}

private boolean isEmptyEditText() {
        return (!(!numAEditText.getText().toString().trim().isEmpty() && !numBEditText.getText().toString().trim().isEmpty()));
    }

{% endhighlight %}

If the EditTexts are not empty then we are good to calculate the given two numbers. We have four functions which separately calculate the two numbers. Each function is defined as follows.

{% highlight java %}
private double addNumbers() {
        return Double.parseDouble(numAEditText.getText().toString()) + Double.parseDouble(numBEditText.getText().toString());
    }

    private double subNumbers() {
        return Double.parseDouble(numAEditText.getText().toString()) - Double.parseDouble(numBEditText.getText().toString());
    }

    private double divNumbers() {
        return Double.parseDouble(numAEditText.getText().toString()) / Double.parseDouble(numBEditText.getText().toString());
    }

    private double mulNumbers() {
        return Double.parseDouble(numAEditText.getText().toString()) * Double.parseDouble(numBEditText.getText().toString());
    }

    private boolean isEmptyEditText() {
        return (!(!numAEditText.getText().toString().trim().isEmpty() && !numBEditText.getText().toString().trim().isEmpty()));
    }
{% endhighlight %}

Notice that we are parsing the string from EditText to Double. One advantage of using Double is that it handles division by zero automatically. Therefore, there's no need to handle any exceptions by ourselves. When we divide by zero the answer shown will be 'Infinity'.
