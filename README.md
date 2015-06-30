# image-share-function-code
image sharing on android

void share(String nameApp, String imagePath) {
        try
        {
            List<Intent> targetedShareIntents = new ArrayList<Intent>();
            Intent share = new Intent(android.content.Intent.ACTION_SEND);
            share.setType("image/jpeg");
            List<ResolveInfo> resInfo = getPackageManager().queryIntentActivities(share, 0);
            if (!resInfo.isEmpty()){
                for (ResolveInfo info : resInfo) {
                    Intent targetedShare = new Intent(android.content.Intent.ACTION_SEND);
                    targetedShare.setType("image/jpeg"); // put here your mime type
                    if (info.activityInfo.packageName.toLowerCase().contains(nameApp) || info.activityInfo.name.toLowerCase().contains(nameApp)) {
                        targetedShare.putExtra(Intent.EXTRA_SUBJECT, "Sample Photo");
                        targetedShare.putExtra(Intent.EXTRA_TEXT,"This photo is created by App Name");
                        targetedShare.putExtra(Intent.EXTRA_STREAM, Uri.fromFile(new File(imagePath)) );
                        targetedShare.setPackage(info.activityInfo.packageName);
                        targetedShareIntents.add(targetedShare);
                    }
                }
                Intent chooserIntent = Intent.createChooser(targetedShareIntents.remove(0), "Select app to share");
                chooserIntent.putExtra(Intent.EXTRA_INITIAL_INTENTS, targetedShareIntents.toArray(new Parcelable[]{}));
                startActivity(chooserIntent);
            }
        }
        catch(Exception e){
            Log.v("VM", "Exception while sending image on" + nameApp + " " + e.getMessage());
        }
    }
