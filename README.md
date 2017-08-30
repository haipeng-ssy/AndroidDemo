# AndroidDemo
写一个工具集，也像个工程只不过是demo工程



// 图片大小缩放和合并
public static Bitmap transImage(String fromFile,int width, int height)
    {
            Bitmap bitmap = BitmapFactory.decodeFile(fromFile);
            int bitmapWidth = bitmap.getWidth();
            int bitmapHeight = bitmap.getHeight();
            // 缩放图片的尺寸
            float scaleWidth = (float) width / bitmapWidth;
            float scaleHeight = (float) height / bitmapHeight;
            Matrix matrix = new Matrix();
            matrix.postScale(scaleWidth, scaleHeight);
            // 产生缩放后的Bitmap对象
            Bitmap resizeBitmap = Bitmap.createBitmap(bitmap, 0, 0, bitmapWidth, bitmapHeight, matrix, false);

           return resizeBitmap;
    }

    public static Bitmap loadBitmapFromViewAndMerge(Context context, View v, boolean isParent, int left, int top, int width, int height, String imagePath) {

        if (v == null) {
            return null;
        }

        Bitmap screenshot = Bitmap.createBitmap(v.getWidth(), v.getHeight(), Bitmap.Config.ARGB_8888);
        Canvas c = new Canvas(screenshot);
        c.translate(-v.getScrollX(), -v.getScrollY());
        v.draw(c);
        Bitmap noteBitmap = Bitmap.createBitmap(screenshot, DensityUtils.dp2px(context, left), DensityUtils.dp2px(context, top),
                DensityUtils.dp2px(context, width), DensityUtils.dp2px(context, height)); //笔记的图片
        

        Bitmap getBitmap = Bitmap.createBitmap(width, height, Bitmap.Config.ARGB_8888); // temp
        Bitmap originBitmap = transImage(imagePath,width, height); // 传经来的原来图片,转换为笔记图片的大小

        Canvas canvas = new Canvas(getBitmap);
        Rect srcRect = new Rect(0, 0, width, height);
        Rect desctRect = new Rect(0, 0, width, height);
        
        canvas.drawBitmap(originBitmap, srcRect, desctRect, null); // 把原来图片画上去
        canvas.drawBitmap(noteBitmap, srcRect, desctRect, null); //把笔记图片画上去

        //最后得到的图片，图片分辨率不变，笔记放大而相对模糊

        return getBitmap;
    }
