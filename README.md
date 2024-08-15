package hiro.build.vpn;

import android.app.Activity;
import android.content.ActivityNotFoundException;
import android.content.BroadcastReceiver;
import android.content.ClipData;
import android.content.ClipboardManager;
import android.content.ComponentName;
import android.content.Context;
import android.content.DialogInterface;
import android.content.Intent;
import android.content.SharedPreferences;
import android.content.pm.PackageInfo;
import android.content.pm.PackageManager;
import android.content.res.Configuration;
import android.graphics.Color;
import android.net.Uri;
import android.net.VpnService;
import android.os.Build;
import android.os.Bundle;
import android.os.CountDownTimer;
import android.os.Handler;
import android.os.PersistableBundle;
import android.os.SystemClock;
import android.support.annotation.NonNull;
import android.support.annotation.Nullable;
import android.support.design.widget.BottomSheetBehavior;
import android.support.design.widget.FloatingActionButton;
import android.support.v4.content.LocalBroadcastManager;
import android.support.v7.app.AlertDialog;
import android.support.v7.widget.AppCompatRadioButton;
import android.support.v7.widget.LinearLayoutManager;
import android.support.v7.widget.PopupMenu;
import android.support.v7.widget.RecyclerView;
import android.support.v7.widget.Toolbar;
import android.view.Menu;
import android.view.MenuInflater;
import android.view.MenuItem;
import me.dawson.proxyserver.ui.ProxySettings;
import org.json.JSONObject;
import java.io.BufferedReader;
import java.io.DataOutputStream;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;
import java.text.DateFormat;
import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.Locale;
import java.util.concurrent.TimeUnit;
import android.view.View;
import android.view.View.OnClickListener;
import android.widget.Button;
import android.widget.Chronometer;
import android.widget.ImageButton;
import android.widget.ImageView;
import android.widget.Spinner;
import android.widget.TextView;
import android.widget.Toast;
import cn.pedant.SweetAlert.SweetAlertDialog;
import com.google.android.gms.ads.AdListener;
import com.google.android.gms.ads.AdRequest;
import com.google.android.gms.ads.AdView;
import com.google.android.gms.ads.InterstitialAd;
import com.google.android.gms.ads.reward.RewardedVideoAd;
import hiro.build.tunnel.StatisticGraphData;
import hiro.build.tunnel.config.ConfigParser;

import hiro.build.tunnel.config.Settings;
import hiro.build.tunnel.logger.ConnectionStatus;
import hiro.build.tunnel.logger.SkStatus;
import hiro.build.tunnel.tunnel.TunnelManagerHelper;
import hiro.build.tunnel.tunnel.TunnelUtils;
import hiro.build.vpn.SocksHttpMainActivity;
import hiro.build.vpn.activities.BaseActivity;
import hiro.build.vpn.adapter.LogsAdapter;
import hiro.build.vpn.adapter.SpinnerAdapter;
import hiro.build.vpn.util.AESCrypt;
import hiro.build.vpn.util.ConfigUpdate;
import hiro.build.vpn.util.ConfigUtil;
import hiro.build.vpn.util.Utils;
import hiro.build.vpn.view.RippleBackground;
import hiro.vpn.pro.R;
import java.io.File;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.OutputStream;
import java.security.GeneralSecurityException;
import java.util.ArrayList;
import java.util.Timer;
import java.util.TimerTask;
import org.json.JSONException;
import org.json.JSONObject;
import android.util.Log;
import hiro.build.vpn.activities.ConfigGeralActivity;
import android.support.v7.app.AppCompatActivity;

public class SocksHttpMainActivity extends BaseActivity
implements View.OnClickListener, SkStatus.StateListener
{
	private static final String UPDATE_VIEWS = "MainUpdate";
	public static LogsAdapter mAdapter;
	private Settings mConfig;
	private Toolbar toolbar_main;
	private Handler mHandler;
	private Button starterButton;
	private ImageButton btnMenu; 
    private ImageButton btnTime;
    private ImageButton mandarin;
	private AdView adsBannerView;
	private ConfigUtil config;
	private TextView status;
	private Spinner serverSpinner;
	private SpinnerAdapter serverAdapter;
	private ArrayList<JSONObject> serverList;
	private InterstitialAd mInterstitialAd;
    private RewardedVideoAd rewardedAd; 
	private SweetAlertDialog nops;
	private SweetAlertDialog progressDialog;
	private RecyclerView logList;
	private ImageView iv1;
	private BottomSheetBehavior<View> bottomSheetBehavior;
	private View bshl;
	private long mTimeLeftInMillis;
	private SharedPreferences sp;
	private boolean mTimerRunning = false;
	public static CountDownTimer mCountDownTimer;
	private TextView tvTimeRemain;
	private TextView btnAddTime;
	private static final int START_VPN_PROFILE = 2002;
	private TextView bytesIn;
	private TextView bytesOut; 
    private Timer timer; 
    private AppCompatActivity mActivity;
    private Chronometer chronometer; 
    private long pauseOffset;
    private boolean running;
	TextView Date;  
    private RippleBackground rippleBackground; 
    private FloatingActionButton deleteLogs;
    private TextView vencimentoDate;
    private TextView diasRestantes;
    private TextView avisos;
    private TextView usuario;
    private String getuser;
    private String URLServer = "http://137.184.211.62:6888/checkUser";
	private String[] torrentList = new String[] {
	/*	"com.termux",
		"com.tdo.showbox",
		"com.nitroxenon.terrarium",
		"com.pklbox.translatorspro",
		"com.xunlei.downloadprovider",
		"com.epic.app.iTorrent",
		"hu.bute.daai.amorg.drtorrent",
		"com.mobilityflow.torrent.prof",
		"com.brute.torrentolite",
		"com.nebula.swift",
		"tv.bitx.media",
		"com.DroiDownloader",
		"bitking.torrent.downloader",
		"jp.co.taosoftware.android.packetcapturepro",
		"com.mobilityflow.tvp",
		"com.gabordemko.torrnado",
		"com.frostwire.android",
		"com.vuze.android.remote",
		"com.akingi.torrent",
		"com.utorrent.web",
		"com.paolod.torrentsearch2",
		"com.delphicoder.flud.paid",
		"com.teeonsoft.ztorrent",
		"megabyte.tdm",
		"com.bittorrent.client.pro",
		"com.mobilityflow.torrent",
		"com.utorrent.client",
		"com.utorrent.client.pro",
		"com.bittorrent.client",
		"torrent",
		"com.AndroidA.DroiDownloader",
		"com.indris.yifytorrents",
		"com.delphicoder.flud",
		"com.oidapps.bittorrent",
		"dwleee.torrentsearch",
		"com.vuze.torrent.downloader",
		"megabyte.dm",
		"com.asantos.vip",
		"com.fgrouptech.kickasstorrents",
		"com.jrummyapps.rootbrowser.classic",
		"com.bittorrent.client",
		"hu.tagsoft.ttorrent.lite",
		"co.we.torrent"*/};

	@Override
    protected void onCreate(@Nullable Bundle savedInstanceState)
    {
        super.onCreate(savedInstanceState);
		 
		new TorrentDetection(this, torrentList).init();
		mHandler = new Handler();
		mConfig = new Settings(this);
		SharedPreferences prefs = getSharedPreferences(SocksHttpApp.PREFS_GERAL, Context.MODE_PRIVATE);
		boolean showFirstTime = prefs.getBoolean("connect_first_time", true);
		if (showFirstTime)
        {
            SharedPreferences.Editor pEdit = prefs.edit();
            pEdit.putBoolean("connect_first_time", false);
            pEdit.apply();
			Settings.setDefaultConfig(this);
			showBoasVindas();
        }
		doLayout();
		//initBytesInAndOut();
		doUpdateLayout(); 
        
        PackageInfo pinfo = Utils.getAppInfo(this);
        if (pinfo != null) {
            String version_nome = pinfo.versionName;
            int version_code = pinfo.versionCode;
            String header_text = String.format("App Version : %s ", version_nome, version_code);

            TextView app_info_text = (TextView) findViewById(R.id.app_info);
            app_info_text.setText(header_text);

        }
        TextView version = (TextView)findViewById (R.id.config_version_info);
		version.setText(config.getVersion());
	}    

    private void initializeStats(){
        timer = new Timer();
        timer.schedule(new TimerTask() {
                @Override
                public void run()
                {
                    runOnUiThread(new Runnable() {
                            @Override
                            public void run() {
                                StatisticGraphData.DataTransferStats stats = StatisticGraphData.getStatisticData().getDataTransferStats();
                                bytesIn.setText(stats.byteCountToDisplaySize(stats.getBytesSent(), false));
                                bytesOut.setText(stats.byteCountToDisplaySize(stats.getBytesReceived(), false));
                            }
                        });
                    // TODO: Implement this method
                }
            },0, 1000);
	} 
    
    private void doLayout() {
        setContentView(R.layout.activity_main_drawer);

        toolbar_main = (Toolbar) findViewById(R.id.toolbar_main);
        
        setSupportActionBar(toolbar_main);
		
		adsBannerView = (AdView) findViewById(R.id.adBannerMainView);
		mInterstitialAd = new InterstitialAd(this);
        mInterstitialAd.setAdUnitId("ca-app-pub-2990234851734594/4904332653");
        mInterstitialAd.loadAd(new AdRequest.Builder().build());

		sp = mConfig.getPrefsPrivate();
		if (TunnelUtils.isNetworkOnline(SocksHttpMainActivity.this)) {
			adsBannerView.setAdListener(new AdListener() {
					@Override
					public void onAdLoaded() {
						if (adsBannerView != null) {
							adsBannerView.setVisibility(View.VISIBLE);
						}
					}
				});
			adsBannerView.loadAd(new AdRequest.Builder()
								 .build());
		}
		
		starterButton = (Button) findViewById(R.id.activity_starterButtonMain);
		starterButton.setOnClickListener(this);
		btnMenu = (ImageButton) findViewById(R.id.btnMenu);
		btnMenu.setOnClickListener(this); 
        btnTime = (ImageButton) findViewById(R.id.btnTime);
		btnTime.setOnClickListener(this);
        
        
        
        
        
        
        btnMenu = (ImageButton) findViewById(R.id.mandarin);
		btnMenu.setOnClickListener(this); 
        
        
        
        
        
        
		status = (TextView) findViewById(R.id.monsour_stats);
		config = new ConfigUtil(this);
		serverSpinner = (Spinner) findViewById(R.id.serverSpinner);
		serverSpinner.setPrompt("Select Server");
		serverSpinner.setPopupBackgroundResource(R.drawable.hiro_alert_bg);
		serverList = new ArrayList<>();
		serverAdapter = new SpinnerAdapter(this, R.id.serverSpinner, serverList);
		serverSpinner.setAdapter(serverAdapter); 
        rippleBackground = (RippleBackground) findViewById(R.id.content);
		loadServer();
		updateConfig(true); 
        
        bytesIn = (TextView) findViewById(R.id.bytesIn);
        bytesOut = (TextView) findViewById(R.id.bytesOut);
		SharedPreferences sPrefs = mConfig.getPrefsPrivate();
		sPrefs.edit().putBoolean(Settings.PROXY_USAR_DEFAULT_PAYLOAD, false).apply();
		sPrefs.edit().putInt(Settings.TUNNELTYPE_KEY, Settings.bTUNNEL_TYPE_SSH_PROXY).apply();
		TextView configVer = (TextView) findViewById(R.id.config_v);
		configVer.setText(config.getVersion());
		View bottomSheet = findViewById(R.id.bottom_sheet); 
		this.bottomSheetBehavior = BottomSheetBehavior.from(bottomSheet);
		this.bshl = findViewById(R.id.bshl);
		this.iv1 = (ImageView)findViewById(R.id.ivLogsDown);
		bshl.setOnClickListener(new OnClickListener(){
				@Override
				public void onClick(View p1)
				{
					if (bottomSheetBehavior.getState() == BottomSheetBehavior.STATE_COLLAPSED) {
						bottomSheetBehavior.setState(BottomSheetBehavior.STATE_EXPANDED);
						iv1.animate().setDuration(500).rotation(180);
					} 
					if(bottomSheetBehavior.getState() == BottomSheetBehavior.STATE_EXPANDED){
						bottomSheetBehavior.setState(BottomSheetBehavior.STATE_COLLAPSED);
						iv1.animate().setDuration(500).rotation(0);
					}
				}
			});

		bottomSheetBehavior.setBottomSheetCallback(new BottomSheetBehavior.BottomSheetCallback() { 
				@Override 
				public void onStateChanged(@NonNull View view, int i) { 
					switch (i){ 
						case BottomSheetBehavior.STATE_COLLAPSED: 
							bottomSheetBehavior.setState(BottomSheetBehavior.STATE_COLLAPSED);
							iv1.animate().setDuration(500).rotation(0);
							break; 
						case BottomSheetBehavior.STATE_EXPANDED: 
							bottomSheetBehavior.setState(BottomSheetBehavior.STATE_EXPANDED);
							iv1.animate().setDuration(300).rotation(180);
							break; 
						case BottomSheetBehavior.STATE_HIDDEN: 
							bottomSheetBehavior.setState(BottomSheetBehavior.STATE_COLLAPSED);
							if (iv1.getRotation() == 0) {
								iv1.animate().setDuration(500).rotation(180);
							} else {
								iv1.animate().setDuration(500).rotation(0);
							}
							break;
					} 
				} 
				@Override 
				public void onSlide(@NonNull View view, float v) { 

				} 
			});
		chronometer = (Chronometer) findViewById(R.id.chronometer);
        chronometer.setFormat("%s");
        chronometer.setBase(SystemClock.elapsedRealtime());
        chronometer.setOnChronometerTickListener(new Chronometer.OnChronometerTickListener() {
                @Override
                public void onChronometerTick(Chronometer chronometer) {
                    if ((SystemClock.elapsedRealtime() - chronometer.getBase()) >= 1440000) {
                        chronometer.setBase(SystemClock.elapsedRealtime());
                        //Toast.makeText(SocksHttpMainActivity.this, "Bing!", Toast.LENGTH_SHORT).show();
                    }
                }
            });
        
		LinearLayoutManager layoutManager = new LinearLayoutManager(this);
		mAdapter = new LogsAdapter(layoutManager, this);
		logList = (RecyclerView) findViewById(R.id.recyclerLog);
		logList.setAdapter(mAdapter);
		logList.setLayoutManager(layoutManager); 
        deleteLogs = (FloatingActionButton)findViewById(R.id.clearLog);
		mAdapter.scrollToLastPosition(); 
        deleteLogs.setOnClickListener(new OnClickListener() {

                @Override
                public void onClick(View p1)
                {
                    mAdapter.clearLog();
                    //  SkStatus.logInfo("<font color='red'>Log Cleared!</font>");
                    // TODO: Implement this method
                }


			});
		boolean isRunning = SkStatus.isTunnelActive();
        if (isRunning) {
            serverSpinner.setEnabled(false);
        } else {
            serverSpinner.setEnabled(true);
        }
	} 
    
    public void monsa1(View v)
    {
        
    }
	
	void showToast(String s) {
		Toast.makeText(this,s,0).show();
	}

	private void doUpdateLayout() {
		SharedPreferences prefs = mConfig.getPrefsPrivate();

		boolean isRunning = SkStatus.isTunnelActive();
		int tunnelType = prefs.getInt(Settings.TUNNELTYPE_KEY, Settings.bTUNNEL_TYPE_SSH_DIRECT);

		setStarterButton(starterButton, this);
		
		switch (tunnelType) {
			case Settings.bTUNNEL_TYPE_SSH_DIRECT:
				((AppCompatRadioButton) findViewById(R.id.activity_mainSSHDirectRadioButton))
					.setChecked(true);
				break;

			case Settings.bTUNNEL_TYPE_SSH_PROXY:
				((AppCompatRadioButton) findViewById(R.id.activity_mainSSHProxyRadioButton))
					.setChecked(true);
				break;
		}
		
        if (isRunning) {
            serverSpinner.setEnabled(false);
        } else {
            serverSpinner.setEnabled(true);
        }
	}
	
	private synchronized void doSaveData() {
		try {
			SharedPreferences prefs = mConfig.getPrefsPrivate();
			SharedPreferences.Editor edit = prefs.edit();

			if (!prefs.getBoolean(Settings.CONFIG_PROTEGER_KEY, false)) {
				if (!prefs.getBoolean(Settings.PROXY_USAR_DEFAULT_PAYLOAD, true)) {
					int pos = serverSpinner.getSelectedItemPosition();
					int tun = config.getServersArray().getJSONObject(pos).getInt("TunnelType"); 
                    String info = config.getServersArray().getJSONObject(pos).getString("Info");
					if (tun == 1) {
						prefs.edit().putInt(Settings.TUNNELTYPE_KEY, Settings.bTUNNEL_TYPE_SSH_SSL).apply();
						String sni = config.getServersArray().getJSONObject(pos).getString("SNI");
						edit.putString(Settings.CUSTOM_SNI, sni);
					} else if (tun == 0) { 
                        if (info.equals("direct")) {  
                            prefs.edit().putInt(Settings.TUNNELTYPE_KEY, Settings.bTUNNEL_TYPE_SSH_DIRECT).apply();
                            String payload = config.getServersArray().getJSONObject(pos).getString("Payload");
							edit.putString(Settings.CUSTOM_PAYLOAD_KEY, payload);  
                            }
                        else if (info.equals("slowdns")) { 
                                prefs.edit().putInt(Settings.TUNNELTYPE_KEY, Settings.bTUNNEL_TYPE_SLOWDNS).apply();
                                String chvKey = config.getServersArray().getJSONObject(pos).getString("ProxyIP");
                                String nvKey = config.getServersArray().getJSONObject(pos).getString("ProxyPort");
                                String dnsKey = config.getServersArray().getJSONObject(pos).getString("Payload");

                                edit.putString(Settings.CHAVE_KEY, chvKey);
                                edit.putString(Settings.NAMESERVER_KEY, nvKey);
                                edit.putString(Settings.DNS_KEY, dnsKey);                                                                                                      
                        } else { 
                            prefs.edit().putInt(Settings.TUNNELTYPE_KEY, Settings.bTUNNEL_TYPE_SSH_PROXY).apply();
                            String payload = config.getServersArray().getJSONObject(pos).getString("Payload");
                            edit.putString(Settings.CUSTOM_PAYLOAD_KEY, payload);
                        }
					} else if (tun == 3) { 
                        if (info.equals("sslproxy")) { 
                            prefs.edit().putInt(Settings.TUNNELTYPE_KEY, Settings.bTUNNEL_TYPE_SSL_RP).apply();
                            String payload = config.getServersArray().getJSONObject(pos).getString("Payload");
                            String sni = config.getServersArray().getJSONObject(pos).getString("SNI");
                            edit.putString(Settings.CUSTOM_PAYLOAD_KEY, payload);
                            edit.putString(Settings.CUSTOM_SNI, sni); 
                        } else {
						prefs.edit().putInt(Settings.TUNNELTYPE_KEY, Settings.bTUNNEL_TYPE_SSL_PAYLOAD).apply();
						String payload = config.getServersArray().getJSONObject(pos).getString("Payload");
						String sni = config.getServersArray().getJSONObject(pos).getString("SNI");
						edit.putString(Settings.CUSTOM_PAYLOAD_KEY, payload);
						edit.putString(Settings.CUSTOM_SNI, sni); 
                        }
					}
				}
			}

			edit.apply();
		} catch (Exception e) {
			SkStatus.logInfo(e.getMessage());
		}
	}
	
	private void loadServerData() {
		try {
			SharedPreferences prefs = mConfig.getPrefsPrivate();
			SharedPreferences.Editor edit = prefs.edit();
			int pos1 = serverSpinner.getSelectedItemPosition();
			int tun = config.getServersArray().getJSONObject(pos1).getInt("TunnelType"); 
            String info = config.getServersArray().getJSONObject(pos1).getString("Info");
            
            if (tun == 1) {
                String ssl_port = config.getServersArray().getJSONObject(pos1).getString("SSLPort");
                edit.putString(Settings.SERVIDOR_PORTA_KEY, ssl_port);
            } else if (tun == 0) { 
                if (info.equals("slowdns")) {  
                    edit.putString(Settings.SERVIDOR_KEY, "127.0.0.1");
                    edit.putString(Settings.SERVIDOR_PORTA_KEY, "2222");
                } else {              
                String ssh_port = config.getServersArray().getJSONObject(pos1).getString("ServerPort");
                edit.putString(Settings.SERVIDOR_PORTA_KEY, ssh_port); 
                }
            } else if (tun == 3) {
				String ssl_port = config.getServersArray().getJSONObject(pos1).getString("SSLPort");
                edit.putString(Settings.SERVIDOR_PORTA_KEY, ssl_port);
			}

			String ssh_server = config.getServersArray().getJSONObject(pos1).getString("ServerIP");
			String remote_proxy = config.getServersArray().getJSONObject(pos1).getString("ProxyIP");
			String proxy_port = config.getServersArray().getJSONObject(pos1).getString("ProxyPort");
            //String ssh_user = config.getServersArray().getJSONObject(pos1).getString("ServerUser");
            //String ssh_password = config.getServersArray().getJSONObject(pos1).getString("ServerPass");

            //edit.putString(Settings.USUARIO_KEY, ssh_user);
            //edit.putString(Settings.SENHA_KEY, ssh_password); 
            
            String hadwerid = android.provider.Settings.Secure.getString(getContentResolver(), android.provider.Settings.Secure.ANDROID_ID);        
            edit.putString(Settings.USUARIO_KEY, hadwerid);
			edit.putString(Settings.SENHA_KEY, "Wilfred");
			edit.putString(Settings.SERVIDOR_KEY, ssh_server);
			edit.putString(Settings.PROXY_IP_KEY, remote_proxy);
			edit.putString(Settings.PROXY_PORTA_KEY, proxy_port);

			edit.apply();

		} catch (Exception e) {
			SkStatus.logInfo(e.getMessage());
		}
	}

	private void loadServer() {
		try {
			if (serverList.size() > 0) {
				serverList.clear();
				serverAdapter.notifyDataSetChanged();
			}
			for (int i = 0; i < config.getServersArray().length(); i++) {
				JSONObject obj = config.getServersArray().getJSONObject(i);
				serverList.add(obj);
				serverAdapter.notifyDataSetChanged();

			}
		} catch (Exception e) {
			e.printStackTrace();
		}
	}

    
    
    
    
    
    
    

    
    
    
    //ChecUser vps manager creditos @GATESXCN
    private void CheckUser() {
        final SharedPreferences prefs= mConfig.getPrefsPrivate();
        getuser = prefs.getString(Settings.USUARIO_KEY, ""); //AQUI É ONDE PEGA O USUÁRIO USADO
        diasRestantes = (TextView) findViewById(R.id.txtdiasRestantes);
        vencimentoDate = (TextView) findViewById(R.id.txtVencimiento) ;
        avisos= (TextView) findViewById(R.id.txtAvisos) ;
        usuario = (TextView) findViewById(R.id.txtHwid);
        usuario.setVisibility(View.VISIBLE);
        usuario.setText(getuser);
        Thread thread = new Thread(new Runnable() {
                @Override
                public void run() {
                    try {
                        URL url = new URL(URLServer);
                        HttpURLConnection conn = (HttpURLConnection) url.openConnection();
                        conn.setRequestMethod("POST");
                        conn.setRequestProperty("Content-Type", "application/json;charset=UTF-8");
                        conn.setRequestProperty("Accept","application/json");
                        conn.setDoOutput(true);
                        conn.setDoInput(true);

                        JSONObject jsonParam = new JSONObject();
                        jsonParam.put("user", getuser);

                        Log.i("JSON", jsonParam.toString());

                        DataOutputStream os = new DataOutputStream(conn.getOutputStream());
                        //os.writeBytes(URLEncoder.encode(jsonParam.toString(), "UTF-8"));
                        os.writeBytes(jsonParam.toString());
                        os.flush();
                        os.close();

                        if (conn.getResponseCode() == HttpURLConnection.HTTP_OK) {
                            try (BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(conn.getInputStream()))) {
                                String result;

                                while ((result = bufferedReader.readLine()) != null) {
                                    String resultFinal = result.replace("\"", "");
                                    if (resultFinal.equals("not exist")){
                                        runOnUiThread(new Runnable() {
                                                @Override
                                                public void run() {
                                                    vencimentoDate.setVisibility(View.GONE);
                                                    usuario.setVisibility(View.GONE);
                                                    diasRestantes.setVisibility(View.GONE);
                                                    avisos.setVisibility(View.VISIBLE);
                                                    avisos.setText("Tu User No Existe!");
                                                    avisos.setTextColor(Color.RED);
                                                }
                                            });

                                    }else{
                                        showUserInfo(resultFinal);
                                    }
                                    Log.d("JSON",resultFinal);

                                }
                            }
                        }else {
                            vencimentoDate.setVisibility(View.GONE);
                            usuario.setVisibility(View.GONE);
                            diasRestantes.setVisibility(View.GONE);
                            avisos.setVisibility(View.VISIBLE);
                            avisos.setText("Error Contacte A El Mandarin Sniff :(");
                            avisos.setTextColor(Color.RED);
                            //ERRO NA CHECAGEM
                            SkStatus.logInfo("No configurado");
                        }

                        conn.disconnect();
                    } catch (Exception e) {
                        e.printStackTrace();
                    }
                }
            });

        thread.start();
    }

    private void showUserInfo(final String result) {
        //MOSTRA DATA E FAZ CHECAGEM
        final SimpleDateFormat sdf = new SimpleDateFormat("dd/MM/yyyy");
        DateFormat dateFormat = new SimpleDateFormat("dd/MM/yyyy");
        Date hoje = new Date();
        final String CurrentDate = dateFormat.format(hoje);

        runOnUiThread(new Runnable() {
                @Override
                public void run() {
                    //FORMATA DATA
                    String data = formatDate(result);
                    //SETA DATA DE VENCIMENTO FORMATADA NO TEXTVIEW
                    vencimentoDate.setVisibility(View.VISIBLE);
                    vencimentoDate.setText("Vence "+data);

                    //VERIFICA DIAS RESTANTES
                    try {
                        Date firstDate = sdf.parse(CurrentDate);
                        Date secondDate = sdf.parse(data);

                        long diff = secondDate.getTime() - firstDate.getTime();
                        TimeUnit time = TimeUnit.DAYS;
                        long dias_diferenca = time.convert(diff, TimeUnit.MILLISECONDS);
                        int dias_diferenca_int = (int) dias_diferenca;

                        diasRestantes.setVisibility(View.VISIBLE);
                        diasRestantes.setText("Dias restantes: " + dias_diferenca_int);

                        avisos.setVisibility(View.VISIBLE);
                        if (dias_diferenca_int <= 3){
                            avisos.setText("Atencion "+ dias_diferenca_int + " Renueva Con Anticipacion");
                            avisos.setTextColor(Color.RED);

                            avisos.setOnClickListener(new View.OnClickListener() {

                                    @Override
                                    public void onClick(View v) {
                                        String url = "";

                                        Intent intentsuporte = new Intent(Intent.ACTION_VIEW, Uri.parse(url));
                                        intentsuporte.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
                                        startActivity(Intent.createChooser(intentsuporte, "Abrir com"));
                                    }
                                });
                        }else{
                            avisos.setText("Login Activo :)");
                            avisos.setTextColor(Color.BLUE);

                        }

                    } catch (Exception e) {
                        e.printStackTrace();
                    }

                }
            });




    }

    public static String formatDate(String data){
        String dateStr = data;
        String returnString = "";
        try{
            SimpleDateFormat sdf = new SimpleDateFormat("ddMMyyyy");
            Date date = sdf.parse(dateStr);
            sdf = new SimpleDateFormat("dd/MM/yyyy");
            dateStr = sdf.format(date);
            returnString = dateStr;

        } catch (Exception e) {
            e.printStackTrace();
        }
        return returnString;
    }
    
    
    
	private void updateConfig(final boolean isOnCreate) {
		new ConfigUpdate(this, new ConfigUpdate.OnUpdateListener() {
				@Override
				public void onUpdateListener(String result) {
					try {
						if (!result.contains("Error on getting data")) {
							String json_data = AESCrypt.decrypt(config.PASSWORD, result);
							if (isNewVersion(json_data)) {
								newUpdateDialog(result);
							} else {
								if (!isOnCreate) {
									noUpdateDialog();
								}
							}
						} else if(result.contains("Error on getting data") && !isOnCreate){
							errorUpdateDialog(result);
						}
					} catch (Exception e) {
						SkStatus.logInfo(e.getMessage());
					}
				}
			}).start(isOnCreate);
	}

	private boolean isNewVersion(String result) {
		try {
			String current = config.getVersion();
			String update = new JSONObject(result).getString("Version");
			return config.versionCompare(update, current);
		} catch (JSONException e) {
			e.printStackTrace();
		}
		return false;
	}

	private void newUpdateDialog(final String result) throws JSONException, GeneralSecurityException {
		String json_data = AESCrypt.decrypt(config.PASSWORD, result);
		String notes = new JSONObject(json_data).getString("ReleaseNotes");
		new AlertDialog.Builder(SocksHttpMainActivity.this, R.style.HiroDialog)
			.setTitle("¡𝗡𝘂𝗲𝘃𝗮 𝗔𝗰𝘁𝘂𝗮𝗹𝗶𝘇𝗮𝗰𝗶ó𝗻 𝗗𝗶𝘀𝗽𝗼𝗻𝗶𝗯𝗹𝗲!")
			.setMessage("\n 🌀 Para tener las nuevas conexiones a internet precione el boton ACTUALIZAR. \n\n 🌀 Recuerde que una ves que acepté la actualización debe cerrar la aplicación. \n\n 🌀 𝗡𝗢𝗧𝗔 , si no acepta la actualización puede tener problemas con la conexión a internet. \n\n 🌀 DISTRIBIDOR OFICIAL  DE VENTA \n\n ࿇ ══━ wilfredito_net ━══ ࿇  \n ⬤ WhatsApp: 75436585  \n\n    ༺🐉 @𝘄𝗶𝗹𝗳𝗿𝗲𝗱𝗶𝘁𝗼_𝗻𝗲𝘁 🐉༻ ")
			.setPositiveButton("Actualizar", new DialogInterface.OnClickListener() {
				@Override
				public void onClick(DialogInterface dialogInterface, int i) {
					try {
						File file = new File(getFilesDir(), "Config");
						OutputStream out = new FileOutputStream(file);
						out.write(result.getBytes());
						out.flush();
						out.close();
						restart_app();
					} catch (IOException e) {
						e.printStackTrace();
					}
				}
			})
			.setNegativeButton("Cancelar", null)
			.create().show();
	}

	private void noUpdateDialog() {
		new AlertDialog.Builder(SocksHttpMainActivity.this, R.style.HiroDialog)
			.setTitle("𝗔𝗰𝘁𝘂𝗮𝗹𝗶𝘇𝗮𝗰𝗶ó𝗻 𝗻𝗼 𝗗𝗶𝘀𝗽𝗼𝗻𝗶𝗯𝗹𝗲")
			.setMessage("Su configuración está en la última versión")
			.setPositiveButton("Ok",null)
			.create().show();
	}

	private void errorUpdateDialog(String error) {
		new AlertDialog.Builder(SocksHttpMainActivity.this, R.style.HiroDialog)
			.setTitle("Error en la actualización")
			.setMessage("Ocurrió un error al buscar actualizaciones.\nPor favor, póngase en contacto con el desarrollador.")
			.setPositiveButton("Ok", null)
			.create().show();
	}

	private void restart_app() {
		Context context = SocksHttpApp.getApp();
		PackageManager packageManager = context.getPackageManager();
		Intent intent = packageManager.getLaunchIntentForPackage(context.getPackageName());
		ComponentName componentName = intent.getComponent();
		Intent mainIntent = Intent.makeRestartActivityTask(componentName);
		context.startActivity(mainIntent);
		Runtime.getRuntime().exit(0);
	}

	public void startOrStopTunnel(Activity activity) {
		if (SkStatus.isTunnelActive()) {
            SharedPreferences prefs = mConfig.getPrefsPrivate();
			TunnelManagerHelper.stopSocksHttp(activity);  
            starterButton.setBackgroundDrawable(getDrawable(R.drawable.power));
            pauseChronometer();
            resetChronometer(); 
            if(bottomSheetBehavior.getState() == BottomSheetBehavior.STATE_EXPANDED){
                bottomSheetBehavior.setState(BottomSheetBehavior.STATE_COLLAPSED);
                iv1.animate().setDuration(500).rotation(0);
            }
		}
		else {
			launchVPN(); 
            startChronometer();   
            if (bottomSheetBehavior.getState() == BottomSheetBehavior.STATE_COLLAPSED) {
                bottomSheetBehavior.setState(BottomSheetBehavior.STATE_EXPANDED);
                iv1.animate().setDuration(500).rotation(180);
            }
		}
	}

	private void launchVPN() {
		Intent intent = VpnService.prepare(this); 
        
        if (intent != null) {
            SkStatus.updateStateString("USER_VPN_PERMISSION", "", R.string.state_user_vpn_permission,
									   ConnectionStatus.LEVEL_WAITING_FOR_USER_INPUT);
            // Start the query
            try {
                startActivityForResult(intent, START_VPN_PROFILE);
            } catch (ActivityNotFoundException ane) {
                SkStatus.logError(R.string.no_vpn_support_image);
            }
        } else {
            onActivityResult(START_VPN_PROFILE, Activity.RESULT_OK, null);
        }
    }

	@Override
	protected void onActivityResult(int requestCode, int resultCode, Intent data) {
		super.onActivityResult(requestCode, resultCode, data);
		if (requestCode == START_VPN_PROFILE) {
            if (resultCode == Activity.RESULT_OK) {
				SharedPreferences prefs = mConfig.getPrefsPrivate();

				if (!TunnelUtils.isNetworkOnline(this)) {
					Toast.makeText(this, "No Internet Connection", 0).show();
				} else
					TunnelManagerHelper.startSocksHttp(this);
			}
		}}

	public void setStarterButton(Button starterButton, Activity activity) {
		String state = SkStatus.getLastState();
		boolean isRunning = SkStatus.isTunnelActive();

		if (starterButton != null) {
			int resId;

			SharedPreferences prefsPrivate = new Settings(activity).getPrefsPrivate();

			if (ConfigParser.isValidadeExpirou(prefsPrivate
											   .getLong(Settings.CONFIG_VALIDADE_KEY, 0))) {
				resId = R.string.expired;
				starterButton.setEnabled(false);

				if (isRunning) {
					startOrStopTunnel(activity);
				}
			}
			else if (prefsPrivate.getBoolean(Settings.BLOQUEAR_ROOT_KEY, false) &&
					 ConfigParser.isDeviceRooted(activity)) {
				resId = R.string.blocked;
				starterButton.setEnabled(false);

				Toast.makeText(activity, R.string.error_root_detected, Toast.LENGTH_SHORT)
					.show();

				if (isRunning) {
					startOrStopTunnel(activity);
				}
			}
			else if (SkStatus.SSH_INICIANDO.equals(state)) {
				resId = R.string.stop;
				starterButton.setEnabled(false);
			}
			else if (SkStatus.SSH_PARANDO.equals(state)) {
				resId = R.string.state_stopping;
				starterButton.setEnabled(false);
				StatisticGraphData.getStatisticData().getDataTransferStats().stop();                
			}
			else {
				resId = isRunning ? R.string.stop : R.string.start;
				starterButton.setEnabled(true);
			}
			//starterButton.setText(resId);
		}
	}

    public void resetChronometer() {
        chronometer.setBase(SystemClock.elapsedRealtime());
        pauseOffset = 0;
    }
    public void startChronometer() {
        if (!running) {
            chronometer.setBase(SystemClock.elapsedRealtime() - pauseOffset);
            chronometer.start();
            running = true;
        }
    }
    public void pauseChronometer() {
        if (running) {
            chronometer.stop();
            pauseOffset = SystemClock.elapsedRealtime() - chronometer.getBase();
            running = false;
        }
    }

	@Override
    public void onPostCreate(Bundle savedInstanceState, PersistableBundle persistentState) {
        super.onPostCreate(savedInstanceState, persistentState);
     
    }

    @Override
    public void onConfigurationChanged(Configuration newConfig) {
        super.onConfigurationChanged(newConfig);
        
    }

	@Override
	public void onClick(View p1)
	{
		switch (p1.getId()) {
			case R.id.activity_starterButtonMain:
				doSaveData();
				mAdapter.clearLog();
				loadServerData();
				startOrStopTunnel(this);
				break;
			case R.id.btnMenu:
				showMenu();
				break;
            case R.id.btnTime:
                fun1();
				break; 
            case R.id.mandarin:
                String official12 = "https://t.me/gatesccn";
                Intent cintent12 = new Intent("android.intent.action.VIEW", Uri.parse(official12));
                startActivity(cintent12);
                break; 
            
		}
	} 
    
   public void fun1() {  
   showBoasVindas();
    }
	
	void showMenu() {
		PopupMenu popup = new PopupMenu(this, btnMenu);
		MenuInflater inflater = popup.getMenuInflater();
		inflater.inflate(R.menu.main_menu, popup.getMenu());
		popup.setOnMenuItemClickListener(new PopupMenu.OnMenuItemClickListener() {

                private Intent cintent10;
				@Override
				public boolean onMenuItemClick(MenuItem p1)
				{
					switch (p1.getItemId()) {
						case R.id.configUpdate:
							updateConfig(false);
							break;
						case R.id.contactot:
							String official = "https://t.me/gatesccn";
							Intent cintent = new Intent("android.intent.action.VIEW", Uri.parse(official));
								startActivity(cintent);
								
							break;
                        
						case R.id.hostshare:
							Intent hostshare2 = new Intent(SocksHttpMainActivity.this, ProxySettings.class);
							startActivity(hostshare2);
							break;
						case R.id.canalt:
							String official1 = "https://t.me/gatesccn";
							Intent cintent1 = new Intent("android.intent.action.VIEW", Uri.parse(official1));
							startActivity(cintent1);
							break;
						case R.id.grupot:
							String official2 = "https://t.me/gatesccn";
							Intent cintent2 = new Intent("android.intent.action.VIEW", Uri.parse(official2));
							startActivity(cintent2);
							break;
                        
                        
						case R.id.contactow:
							String official3 = "https://t.me/gatesccn";
							Intent cintent3 = new Intent("android.intent.action.VIEW", Uri.parse(official3));
							startActivity(cintent3);
							break;
						case R.id.grupow:
							String official4 = "https://t.me/gatesccn";
							Intent cintent4 = new Intent("android.intent.action.VIEW", Uri.parse(official4));
							startActivity(cintent4);
							break;
						case R.id.youtube:
							String official5 = "https://t.me/gatesccn";
							Intent cintent5 = new Intent("android.intent.action.VIEW", Uri.parse(official5));
							startActivity(cintent5);
							break;
						case R.id.facebook:
							String official6 = "https://t.me/gatesccn";
							Intent cintent6 = new Intent("android.intent.action.VIEW", Uri.parse(official6));
							startActivity(cintent6);
							break;
						
						case R.id.miExit:
							showExitDialog();
							break; 
                        case R.id.cop:
                            String idH = android.provider.Settings.Secure.getString(getContentResolver(),android.provider.Settings.Secure.ANDROID_ID);
                            String hadweridr = (idH);  
                            
                            new AlertDialog.Builder(SocksHttpMainActivity.this, R.style.HiroDialog)
                                .setTitle("𝗧𝗼𝗸𝗲𝗻 𝗜𝗗 𝗨𝗻𝗶𝗰𝗼")
                                .setMessage(hadweridr)
                                .setPositiveButton(R.string.token, new DialogInterface.OnClickListener() {
                                    @Override
                                    public void onClick(DialogInterface di, int p) {
                                        // ok
                                    }
                                })
                                .setCancelable(false)
                                .show();
                                                         
                            ClipboardManager clipboard = (ClipboardManager) getSystemService(Context.CLIPBOARD_SERVICE);
                            ClipData clip = ClipData.newPlainText("text",  hadweridr);
                            clipboard.setPrimaryClip(clip);
                            break;
					}
					return true;
				}
			});
		popup.show();
	}

	protected void showBoasVindas() {
		new AlertDialog.Builder(this, R.style.HiroDialog)
            . setTitle(R.string.sobre)
            . setMessage("Esta aplicación fue desarrollada para usuarios avanzados, tenga esto en cuenta al usarla. \n\n Soporte 24/7 para clientes vip sin importar en el pais que se encuentre \n\n ● DESARROLLADOR ● \n\n 𝑩𝒚: EL MANDARIN SNIFF")
			. setPositiveButton(R.string.ok, new DialogInterface.OnClickListener() {
				@Override
				public void onClick(DialogInterface di, int p) {
					// ok
				}
			})
			. setCancelable(false)
            . show();
	}

	@Override
	public void updateState(final String state, String msg, int localizedResId, final ConnectionStatus level, Intent intent)
	{
		mHandler.post(new Runnable() {
				@Override
				public void run() {
					doUpdateLayout();
					if(SkStatus.isTunnelActive()){  
						if(level.equals(ConnectionStatus.LEVEL_CONNECTED)) { 
                            status.setTextColor(Color.CYAN);
							status.setText(R.string.state_connected);
						}
						if(level.equals(ConnectionStatus.LEVEL_NOTCONNECTED)){
                            status.setTextColor(Color.RED);
							status.setText(R.string.state_disconnected); 
                            starterButton.setBackgroundDrawable(getDrawable(R.drawable.power));
                            CheckUser();
						}
						if(level.equals(ConnectionStatus.LEVEL_CONNECTING_SERVER_REPLIED)){ 
                            status.setTextColor(Color.WHITE);
							status.setText( R.string.state_auth);
                            CheckUser();
						}
						if(level.equals(ConnectionStatus.LEVEL_CONNECTING_NO_SERVER_REPLY_YET)){ 
                            status.setTextColor(Color.WHITE);
                            CheckUser();
							status.setText(R.string.state_connecting);                            
						}
						if(level.equals(ConnectionStatus.UNKNOWN_LEVEL)){ 
                            status.setTextColor(Color.RED);
                            CheckUser();
							status.setText(R.string.state_disconnected); 
                            CheckUser();
                            starterButton.setBackgroundDrawable(getDrawable(R.drawable.power));
						}
						//if(level.equals(ConnectionStatus.LEVEL_RECONNECTANDO)){
						//status.setText(R.string.state_reconnecting); 
                        
					}
					if(level.equals(ConnectionStatus.LEVEL_NONETWORK)){
                        CheckUser();
						status.setText(R.string.state_nonetwork);
					}
					if(level.equals(ConnectionStatus.LEVEL_AUTH_FAILED)){
                        CheckUser();
						status.setText(R.string.state_auth_failed);
					}

				}
			});

		switch (state) {
			case SkStatus.SSH_CONECTADO: 
                starterButton.setBackgroundDrawable(getDrawable(R.drawable.pwon));
                CheckUser();
				// carrega ads banner
				if (adsBannerView != null && TunnelUtils.isNetworkOnline(SocksHttpMainActivity.this)) {
					adsBannerView.setAdListener(new AdListener() {
							@Override
							public void onAdLoaded() {
								if (adsBannerView != null && !isFinishing()) {
									adsBannerView.setVisibility(View.VISIBLE);
								}
							}
						});
					adsBannerView.postDelayed(new Runnable() {
							@Override
							public void run() {
								// carrega ads interestitial
								AdsManager.newInstance(getApplicationContext())
									.loadAdsInterstitial();
								// ads banner
								if (adsBannerView != null && !isFinishing()) {
									adsBannerView.loadAd(new AdRequest.Builder()
														 .build());
								}
							}
						}, 5000);
				}
				break;
		}
	}


	/**
	 * Recebe locais Broadcast
	 */

	private BroadcastReceiver mActivityReceiver = new BroadcastReceiver() {
        @Override
        public void onReceive(Context context, Intent intent) {
            String action = intent.getAction();
            if (action == null)
                return;

            if (action.equals(UPDATE_VIEWS) && !isFinishing()) {
				doUpdateLayout();
			}
        }
    };


	@Override
    public boolean onCreateOptionsMenu(Menu menu) {
        // Inflate the menu; this adds items to the action bar if it is present.
        getMenuInflater().inflate(R.menu.main_menu, menu);
        return true;
    }

	@Override
	public boolean onOptionsItemSelected(MenuItem item) {
		switch (item.getItemId()) {
			case R.id.configUpdate:
				updateConfig(false);
				break;
			case R.id.miExit:
				if (Build.VERSION.SDK_INT >= 16) {
					finishAffinity();
				}

				System.exit(0);
				break;
		}

		return super.onOptionsItemSelected(item);
	}

	@Override
	public void onBackPressed() {
		showExitDialog();
	}

	@Override
    public void onResume() {
        super.onResume(); 
        initializeStats(); 
        rippleBackground.stopRippleAnimation();
        rippleBackground.setRippleColor(Color.parseColor(getString(R.color.colorConnected)));
       rippleBackground.startRippleAnimation();       
		SkStatus.addStateListener(this);
		if (sp.getBoolean("resume", false)) {
			if (mTimerRunning) {
                startChronometer();
			}
			sp.edit().putBoolean("resume", false).apply();
		}
        if (adsBannerView != null) {
            adsBannerView.resume();
        }
		SkStatus.addStateListener(this);
		if (adsBannerView != null) {
			adsBannerView.resume();
		}
		new Timer().schedule(new TimerTask()
			{
				@Override
				public void run()
				{
					runOnUiThread(new Runnable()
						{
							@Override
							public void run()
							{
								//updateHeaderCallback();
							}
						});
				}
			}, 0,1000);
    }

	@Override
	protected void onPause()
	{
		super.onPause();
		doSaveData();
		SkStatus.removeStateListener(this);
		if (adsBannerView != null) {
			adsBannerView.pause();
		}
	}

	@Override
	protected void onDestroy()
	{
		super.onDestroy();
		sp.edit().putBoolean("resume", true).apply();
		SkStatus.removeLogListener(mAdapter);
		LocalBroadcastManager.getInstance(this)
			.unregisterReceiver(mActivityReceiver);

		if (adsBannerView != null) {
			adsBannerView.destroy();
		}
	}

	public static void updateMainViews(Context context) {
		Intent updateView = new Intent(UPDATE_VIEWS);
		LocalBroadcastManager.getInstance(context)
			.sendBroadcast(updateView);
	}

	public void showExitDialog() {
		AlertDialog dialog = new AlertDialog.Builder(this, R.style.HiroDialog).
			create();
		dialog.setTitle(getString(R.string.attention));
		dialog.setMessage(getString(R.string.alert_exit));

		dialog.setButton(DialogInterface.BUTTON_POSITIVE, getString(R.
																	string.exit),
			new DialogInterface.OnClickListener() {
				@Override
				public void onClick(DialogInterface dialog, int which)
				{
					Utils.exitAll(SocksHttpMainActivity.this);
				}
			}
		);

		dialog.setButton(DialogInterface.BUTTON_NEGATIVE, getString(R.
																	string.minimize),
			new DialogInterface.OnClickListener() {
				@Override
				public void onClick(DialogInterface dialog, int which) {
					// minimiza app
					Intent startMain = new Intent(Intent.ACTION_MAIN);
					startMain.addCategory(Intent.CATEGORY_HOME);
					startMain.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
					startActivity(startMain);
				}
			}
		);
		dialog.setButton(DialogInterface.BUTTON_NEUTRAL, "Cancelar", new DialogInterface.OnClickListener() {
				@Override
				public void onClick(DialogInterface p1, int p2)
				{
					p1.dismiss();
				}
			});
		dialog.show();
	} 
      
}

