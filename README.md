# justjava
/**
 * IMPORTANT: Make sure you are using the correct package name.
 * This example uses the package name:
 * package com.example.android.justjava
 * If you get an error when copying this code into Android studio, update it to match teh package name found
 * in the project's AndroidManifest.xml file.
 **/

package com.example.justjava;


import android.annotation.SuppressLint;
import android.content.Intent;
import android.net.Uri;
import android.os.Bundle;
import android.support.v7.app.AppCompatActivity;
import android.util.Log;
import android.view.View;
import android.widget.CheckBox;
import android.widget.EditText;
import android.widget.TextView;
import android.widget.Toast;

import com.example.justjava.R;

import java.text.NumberFormat;


/**
 * This app displays an order form to order coffee.
 */
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }

    int quantity = 2;

    /**
     * This method is called when the plus button is clicked.
     */
    public void increment(View view) {
        if (quantity == 100){

            Toast.makeText(this,"You can not have more than 100 cups of coffee",Toast.LENGTH_SHORT).show();

            return;
        }
        quantity = quantity + 1;

        displayQuantity(quantity);


    }

    /**
     * This method is called when the minus button is clicked.
     */
    public void decrement(View view) {
        if (quantity == 1){
            Toast.makeText(this,"You can not have less than 1 cup of coffee",Toast.LENGTH_SHORT).show();

            return;
        }
        quantity = quantity - 1;
        displayQuantity(quantity);


    }


    /**
     * This method is called when order button is clicked.
     */
    public void submitOrder(View view) {
        EditText nameField =(EditText) findViewById(R.id.Enter_name);
        String name = nameField.getText().toString();
        CheckBox whippedCreamCheckBox =(CheckBox)findViewById(R.id.whipped_cream_checkbox);
        CheckBox chocolateCheckBox = (CheckBox) findViewById(R.id.chocolate_cream_checkbox);
        boolean hasWhippedCream = whippedCreamCheckBox.isChecked();
        boolean hasChocolate = chocolateCheckBox.isChecked();
        int price = calculatePrice(hasWhippedCream,hasChocolate);
        String priceMessage = createOrderSummary(price,hasWhippedCream,hasChocolate,name);

        Intent intent = new Intent(Intent.ACTION_SENDTO);
        intent.setData(Uri.parse("mailto:"));// only email apps should handle this
        intent.putExtra(Intent.EXTRA_SUBJECT," Just Java order for " + name);
        intent.putExtra(Intent.EXTRA_TEXT,priceMessage);
            if (intent.resolveActivity(getPackageManager()) != null){
                startActivity(intent);
            }
    }

    /**
     * Create summary of the order.
     *
     * @param price of the order
     * @param addWhippedCream is weather or not the user want whipped cream topping
     * @param  addChocolate is weather or not the user want whipped cream topping
     * @return text summary
     */

    @SuppressLint("StringFormatMatches")
    private String createOrderSummary(int price, boolean addWhippedCream, boolean addChocolate, String displayName) {
        String priceMessage = getString(R.string.order_summary_name,displayName);
        priceMessage +="\n" + getString(R.string.order_summary_whipped_cream,addWhippedCream);
        priceMessage +="\n" + getString(R.string.order_summary_chocolate,addChocolate);
        priceMessage += "\n" + getString(R.string.order_summay_quantity,quantity);
        priceMessage += "\n" + getString(R.string.order_summay_price,
                NumberFormat.getCurrencyInstance().format(price));
        priceMessage += "\n" + getString(R.string.thank_you);
        return priceMessage;

    }

    // @return total price

    private int calculatePrice(boolean addWhippedCream,boolean addChocolate) {
        int basePrice = 5;

        if (addWhippedCream){
            basePrice = basePrice + 1;

        }
        if (addChocolate){
            basePrice = basePrice + 2;

        }
        return quantity * basePrice;

    }


    /**
     * This method displays the given quantity value on the screen.
     */
    private void displayQuantity(int numberOfCoffees) {
        TextView quantityTextView = (TextView) findViewById(R.id.quantity_text_view);
        quantityTextView.setText("" + numberOfCoffees);
    }


    /**
     * this method display the price message
     * @param message
     */


}
