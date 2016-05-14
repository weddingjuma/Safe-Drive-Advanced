
//Launches app upon beacon detection
public class AndroidProximityReferenceApplication extends Application implements BootstrapNotifier {
    private static final String TAG = "AndroidProximityReferenceApplication";
    private RegionBootstrap regionBootstrap;

    public void onCreate() {
        super.onCreate();
        Region region = new Region("com.example.backgroundRegion",
                Identifier.parse("2F234454-CF6D-4A0F-ADF2-F4911BA9FFA6"), null, null);
        regionBootstrap = new RegionBootstrap(this, region);
    }

    @Override
    public void didEnterRegion(Region arg0) {
        Log.d(TAG, "did enter region.");
        Intent intent = new Intent(this, MainActivity.class);
        intent.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK);

        // Important:  make sure to add android:launchMode="singleInstance" in the manifest
        // to keep multiple copies of this activity from getting created if the user has
        // already manually launched the app.
        this.startActivity(intent);
    }
   ...
}

@Override
public void onRequestPermissionsResult(int requestCode,
										   String permissions[], int[] grantResults) {
	switch (requestCode) {
		case PERMISSION_REQUEST_COARSE_LOCATION: {
			if (grantResults[0] == PackageManager.PERMISSION_GRANTED) {
				Log.d(TAG, "coarse location permission granted");
			} else {
				final AlertDialog.Builder builder = new AlertDialog.Builder(this);
				builder.setTitle("Functionality limited");
				builder.setMessage("Since location access has not been granted, this app will not be able to discover beacons when in the background.");
				builder.setPositiveButton(android.R.string.ok, null);
				builder.setOnDismissListener(new DialogInterface.OnDismissListener() {

					@Override
					public void onDismiss(DialogInterface dialog) {
					}

				});
				builder.show();
			}
			return;
		}
	}
}


//promps user for location access
android.permission.ACCESS_COARSE_LOCATION

private static final int PERMISSION_REQUEST_COARSE_LOCATION = 1;
	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);

		...
		
    if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M) {     
        // Android M Permission check     
        if (this.checkSelfPermission(Manifest.permission.ACCESS_COARSE_LOCATION) != PackageManager.PERMISSION_GRANTED) {         
            final AlertDialog.Builder builder = new AlertDialog.Builder(this); 
            builder.setTitle("This app needs location access");
            builder.setMessage("Please grant location access so this app can detect beacons.");
            builder.setPositiveButton(android.R.string.ok, null); 
            builder.setOnDismissListener(new DialogInterface.OnDismissListener() {  
                    @Override 
                    public void onDismiss(DialogInterface dialog) {
                            requestPermissions(new String[]{Manifest.permission.ACCESS_COARSE_LOCATION}, PERMISSION_REQUEST_COARSE_LOCATION);             
                    }  
            }); 
            builder.show();    
        }
     }
}


