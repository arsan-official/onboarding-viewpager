## Onboarding view pager
menampilkan swipe tutorial dengan menggunakan view pager, mengimplementasikan material design style, dan cukup populer digunakan sebagai halaman tutorial aplikasi saat pertama kali dibuka oleh user. 

<img src="https://media.giphy.com/media/xUPGcFkluhGnL08rsI/giphy.gif" />

### Garis Besar Tutorial
1. Buat project dengan memilih Tabbed Activity Template. 
2. Remove toolbar. 
3. Buat layout footer untuk control view pager, terdiri dari: button skip, indicator, button next dan button finish.  
   <img src='https://i.imgur.com/lzFhSA3.png' />  
4. Siapkan fragment yang ingin ditampilkan. 
5. Buat file xml untuk gambar indicator selected dan unselected.  
    * selected shape xml:  
    ```
    <shape xmlns:android="http://schemas.android.com/apk/res/android" android:shape="oval">
      <corners android:radius="100dp" />
      <solid android:color="@android:color/white" />
    </shape>
    ```
    * unselected shape xml:  
    ```
    <shape xmlns:android="http://schemas.android.com/apk/res/android" android:shape="oval">
      <corners android:radius="100dp" />
      <solid android:color="@android:color/transparent" />
      <stroke android:color="@android:color/white" android:width="1dp" />
    </shape>
    ```
6. Simpan image indicator kedalam array: 
   ```
   indicators = new ImageView[]{
                (ImageView) findViewById(R.id.footer_control_indicator_1),
                (ImageView) findViewById(R.id.footer_control_indicator_2),
                (ImageView) findViewById(R.id.footer_control_indicator_3)};
   ```
7. Update background indicator setiap terjadi perubahan posisi view pager:  
   ```
   private void updateIndicator(int position){
        for (int i = 0; i < indicators.length; i++) {
           if(i == position)
                indicators[i].setImageResource(R.drawable.indicator_selected);
           else
                indicators[i].setImageResource(R.drawable.indicator_unselected);
        }
   }
   ```  
8. addPageChangeListener : 
   ```
    @Override
    public void onPageSelected(int position) {
        // update indicator
        updateIndicator(position);
        
        btnSkip.setVisibility(position == 2? View.INVISIBLE: View.VISIBLE);
        btnNext.setVisibility(position == 2? View.GONE : View.VISIBLE);
        btnFinish.setVisibility(position == 2 ? View.VISIBLE : View.GONE);

    }
   ```
9. Buat array untuk menyimpan background color  
   ```
    colorList = new int[]{Color.parseColor("#FFC107"), Color.parseColor("#3F51B5"), Color.parseColor("#8BC34A")};
   ```
10. Buat transisi background color antar page dengan override method onPageScrolled: 
    ```
    @Override
    public void onPageScrolled(int position, float positionOffset, int positionOffsetPixels) {
        int colorUpdate = (Integer) evaluator.evaluate(positionOffset,
                colorList[position], position == colorList.length-1 ? colorList[position] : colorList[position+1]);
        mViewPager.setBackgroundColor(colorUpdate);
    }
    ```
11. selesai
12. Original tutorial: http://blog.iamsuleiman.com/onboarding-android-viewpager-google-way/ 
13. Source code direpository saat telah saya modifikasi sedemikianrupa sehingga tidak 100% sama dengan tutorial aslinya. 
