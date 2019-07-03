# Switch Mediator

The **Switch Mediator** is an XPath or JSONPath filter. The XPath or
JSONPath is evaluated and returns a string. This string is matched
against the regular expression in each switch case mediator, in the
specified order. If a matching case is found, it will be executed, and
the remaining switch case mediators are not processed. If none of the
case statements are matching, and a default case is specified, the
default will be executed.

!!! info

The Switch mediator is a
[conditionally](ESB-Mediators_119131045.html#ESBMediators-Content-awareness)
[content
aware](ESB-Mediators_119131045.html#ESBMediators-Content-awareness)
mediator.


------------------------------------------------------------------------

[Syntax](#SwitchMediator-Syntax) \|
[Configuration](#SwitchMediator-Configuration) \|
[Examples](#SwitchMediator-Examples)

------------------------------------------------------------------------

### Syntax

``` java
    <switch source="[XPath|json-eval(JSON Path)]">
       <case regex="string">
         mediator+
       </case>+
       <default>
         mediator+
       </default>?
    </switch>
```

------------------------------------------------------------------------

### Configuration

The parameters available to configure the Switch mediator are as
follows.

<table>
<thead>
<tr class="header">
<th>Parameter Name</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><strong>Source XPath</strong></td>
<td>The source XPath or JSONPath to be evaluated. When specifying a JSONPath, use the format <code>             json-eval(&lt;JSON_PATH&gt;)            </code> , such as <code>             json-eval(getQuote.request.symbol)            </code> . For more information on using JSON with the the ESB profile , see <a href="https://docs.wso2.com/display/EI650/Working+with+JSON+Message+Payloads">Working with JSON Message Payloads</a> . If you use namespaces in the expression, click <strong>Namespaces</strong> and map the namespace prefix to the correct URI.</td>
</tr>
<tr class="even">
<td><strong>Number of cases</strong></td>
<td><p>This parameter displays the number of cases currently added to the Switch mediator configuration. See <a href="#SwitchMediator-Switch-CaseMediator">Switch-case-mediator</a> for instructions to add a case.</p>
<p><br />
</p></td>
</tr>
<tr class="odd">
<td><strong>Specify default case</strong></td>
<td>Click this link to add a default switch-case mediator. Adding a default switch case mediator is optional. If it is specified, it will be executed if no matching switch-case is identified.</td>
</tr>
</tbody>
</table>

#####  Switch-case mediator

1.  To add a case, click **Add case** , which adds an empty switch-case
    mediator under the Switch mediator. A switch-case mediator would
    appear as a child of the Switch mediator.
2.  Click **Case** to configure the switch-case mediator. The page will
    expand to display the section shown below where a regular expression
    can be added in the **Case Value (Regular Expression)** parameter.
3.  Click **Case** again and click **Add Child** , and add the
    mediator(s) you want to execute when this case matches the switching
    value.

------------------------------------------------------------------------

### Examples

In this example the [Property mediator](http://Property%20Mediator) sets
the local property named `         symbol        ` on the current
message depending on the evaluation of the string. It will get the text
of symbol element and match it against the values
`         MSFT        ` and `         IBM        ` . If the text does
not match either of these symbols, the default case will be executed.

``` java
    <switch source="//m0:getQuote/m0:request/m0:symbol" xmlns:m0="http://services.samples/xsd">
          <case regex="IBM">
              <!-- the property mediator sets a local property on the *current* message -->
              <property name="symbol" value="Great stock - IBM"/>
          </case>
          <case regex="MSFT">
              <property name="symbol" value="Are you sure? - MSFT"/>
          </case>
          <default>
              <!-- it is possible to assign the result of an XPath or JSON Path expression as well -->
              <property name="symbol"
                    expression="fn:concat('Normal Stock - ', //m0:getQuote/m0:request/m0:symbol)"
                    xmlns:m0="http://services.samples/xsd"/>
          </default>
      </switch>
```
