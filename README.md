package com.example.test;

import java.util.ArrayList;
import java.util.List;

import android.app.Activity;
import android.hardware.Sensor;
import android.hardware.SensorEvent;
import android.hardware.SensorEventListener;
import android.hardware.SensorManager;
import android.os.Bundle;
import android.util.Log;
import android.view.Menu;
import android.view.MenuItem;
import android.widget.TextView;


public class MainActivity extends Activity implements SensorEventListener{

	
	public TextView hb, tvAvg, tvList;
	long startTime,currentTime;
	int sum=0,counter=0,avg;
	boolean check=true,start=true;
	List<Float> heartRates;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        hb= (TextView)findViewById(R.id.tvhb);
        tvAvg = (TextView)findViewById(R.id.tvAvgs);
        tvList = (TextView) findViewById(R.id.tvListHeartRates);
        heartRates= new ArrayList<Float>();
        currentTime=System.currentTimeMillis();
        openSensor();
        
       
    }

    public void openSensor(){
    	 SensorManager mSensorManager = ((SensorManager)getSystemService(SENSOR_SERVICE));
    	 Sensor mHeartRateSensor = mSensorManager.getDefaultSensor(Sensor.TYPE_HEART_RATE); 
    	 mSensorManager.registerListener(this, mHeartRateSensor, SensorManager.SENSOR_DELAY_NORMAL);
    	
    	 Log.e("test","openSensor()");
    }

    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        // Inflate the menu; this adds items to the action bar if it is present.
        getMenuInflater().inflate(R.menu.main, menu);
        return true;
    }

    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        // Handle action bar item clicks here. The action bar will
        // automatically handle clicks on the Home/Up button, so long
        // as you specify a parent activity in AndroidManifest.xml.
        int id = item.getItemId();
        if (id == R.id.action_settings) {
            return true;
        }
        return super.onOptionsItemSelected(item);
    }

	@Override
	public void onSensorChanged(SensorEvent event) {
	 
		
		if( event.sensor.getType() == Sensor.TYPE_HEART_RATE){
			counter++;
			
			if((System.currentTimeMillis()-currentTime)>=300000){
				heartRates.add(new Float(event.values[0]));
				currentTime=System.currentTimeMillis();
				tvList.setText(tvList.getText() + " " + event.values[0] + " ");
			}
			
			
			Log.e("test", "1---------");
			String message = "HeartBeat= " + event.values[0];
			Log.e("test","event= " + event);
			sum += event.values[0];
			
		if(start && sum != 0){
			startTime= System.currentTimeMillis();
			start=false;
		}
			if(hb != null)
			{
				hb.setText(message);
				  if ((currentTime-startTime>=300000)&&check==true){
					   avg=sum/counter;
					   tvAvg.setText("avg="+avg);
					   check=false;
				   }
			}
			Log.e("test", "2---------");
		}
		else{
			Log.e("test", "fail sensorChange");
		}
		
	}

	@Override
	public void onAccuracyChanged(Sensor sensor, int accuracy) {
		// TODO Auto-generated method stub
		
	}
}
