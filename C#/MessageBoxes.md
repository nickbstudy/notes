Message boxes can be called with `MessageBox.Show()` and stored in a `DialogResult` type.  First arg is message to be displayed, then the name of the modal, then `MessageBoxButtons.chosenOnes` which can be: `AbortRetryIgnore OK OKCancel RetryCancel YesNo YesNoCancel` and finally `MessageBoxIcon.chosenOne` from:

 - `Error Hand Stop` - any of those are a red X
 - `Question` - Blue question mark
 - `Exclamation Warning` - either is the yellow triangle with an `!`
 - `Asterisk Information` - either is a blue circle with an `i`

 An example of usage:

 ```
try
{
    throw new NotImplementedException();
}
catch (NotImplementedException notImp)
{
    DialogResult result =
        MessageBox.Show("Proceed?", notImp.Message, MessageBoxButtons.YesNo, MessageBoxIcon.Hand);

    textBox1.Text = result.ToString();
    if (result == DialogResult.Yes) { label1.Text = "Proceeding..."; }
    if (result == DialogResult.No) { label1.Text = "Stopping..."; }
}
```