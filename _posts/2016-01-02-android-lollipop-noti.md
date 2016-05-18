---
layout: post
title: 안드로이드 롤리팝 노티피케이션 아이콘 대응
---

안드로이드 롤리팝 버젼에 들어서면서 Custom Notification의 아이콘이 하얗게 오버레이 되는 현상을 겪는 분이 있습니다. 사실 국내는 Kitkat 사용자가 압도적으로 많고 http://developer.android.com/intl/ko/about/dashboards/index.html
![안드로이드 버젼 점유율](http://chart.googleapis.com/chart?chd=t%3A0.2%2C3.0%2C2.7%2C24.7%2C36.1%2C32.6%2C0.7&chf=bg%2Cs%2C00000000&chco=c4df9b%2C6fad0c&chl=Froyo%7CGingerbread%7CIce%20Cream%20Sandwich%7CJelly%20Bean%7CKitKat%7CLollipop%7CMarshmallow&cht=p&chs=500x250)
그래서 쉽게 이런 문제를 알 방법이 없었습니다.(ㅠㅠ) 제 경우엔 개발기기가 넥서스(5.0)인 관계로 노티피케이션 이미지가 SDK 타겟에 따라 다르게 설정되어야 함을 빠르게 알았습니다.

롤리팝 업데이트로 인해서 머터리얼 디자인 스타일의 알림으로 변경되었습니다. 그래서

1. 흰색과 투명색의 이미지만 사용하여야 합니다. 아이콘에 색상이 입혀져 있다면 이는 전부 안드로이드 시스템에 의해 무시되고 흰색으로 보이게 됩니다.
2. setColor를 통해 배경색(악센트 컬러)을 정해야 하므로 흰색 아이콘과 어울리는 배경색을 지정해 주어야 합니다.

그래서 롤리팝과 동시에 하위 버젼을 대응하기 위한 여기 좋은 해결 방법이 있습니다.
http://stackoverflow.com/questions/28387602/notification-bar-icon-turns-white-in-android-5-lollipop

``` java
public void onMessageReceived(...) {
  NotificationCompat.Builder notificationBuilder = new NotificationCompat.Builder(this)
    .setSmallIcon(getNotificationIcon())
    .setContentTitle(title)
    .setContentText(message)
    .setColor(getResources().getColor(R.color.app_color)); // accent color
}
int getNotificationIcon() {
  return android.os.Build.VERSION.SDK_INT >= android.os.Build.VERSION_CODES.LOLLIPOP ? R.drawable.ic_white_icon : R.drawable.ic_launcher_icon;
}
```

저는 이렇게 사용합니다. SDK 22에서 테스트되었습니다.
