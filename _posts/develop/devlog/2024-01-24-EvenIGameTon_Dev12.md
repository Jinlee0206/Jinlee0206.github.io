---
layout: post
title:  "[이븐아이 게임톤] 개발 - 사운드"
excerpt : "개발"
categories: develop
tags: devlog

toc: true

date:   2024-01-24
last_modified_at: 2024-01-25
comments : true
---

* this unordered seed list will be replaced by the toc
{:toc}  


## Sound
  - 사운드 매니저 시스템 구축 완료
  - Intro 씬에서 시작할 때, AudioManager를 싱글톤으로 생성
  - BGM Player는 단일 AudioSource, SFX Players는 복수의 AudioSource로 구성.
  - Inspector 창에서 Sound Engineer가 작업하기 쉽게끔 하나의 AudioManager 하나의 오브젝트에서 조정이 가능하게 끔 구현
<p align="center"> 
  <img src = "https://github.com/Jinlee0206/EvenIGamethon/assets/105345909/f33b4694-7fb8-4966-9dff-ea98882deab6" width = "260" height = "280">
</p> 

  - PopupManager를 이용해서 PopUpWindows를 나타내는 방향으로 PopUpSystem을 구현 하였기 떄문에 SoundSettingUI가 열릴 때, 게임 상의 Sound를 받아오고, 새로 생성된 Slider에 AddListener를 이용해 함수를 연결해주는 작업 완료

## AudioManger.cs

```cs
using UnityEngine;

public class AudioManager : Singleton<AudioManager>
{
    public enum AudioType { BGM, SFX }

    [Header("#BGM")]
    public AudioClip[] bgmClips;                        // BGM 클립 여러개
    public float bgmVolume;
    AudioSource bgmPlayer;                              // BGM 플레이어는 단일

    [Header("#SFX")]
    public AudioClip[] sfxClips;
    public float sfxVolume;
    public int channels;                                // SFX 사운드 채널
    AudioSource[] sfxPlayers;                           // SFX는 동시에 여러개가 실행됨
    int channelIndex;

    public enum BGM { BGM_Title, BGM_Lobby }
    public enum SFX { Dead, Hit, LevelUp = 3, Lose, Melee, Range = 7, Select, Win }

    public float BgmVolume
    {
        get => GetVolume(AudioType.BGM);
        set => OnVolumeChanged(AudioType.BGM, value);
    }

    public float SFXVolume
    {
        get => GetVolume(AudioType.SFX);
        set => OnVolumeChanged(AudioType.SFX, value);
    }

    private void Awake()
    {
        this.Initialize_DontDestroyOnLoad();
        Init();
    }

    private void Init()
    {
        // 배경음 플레이어 초기화
        GameObject bgmObject = new GameObject("BGMPlayer");
        bgmObject.transform.parent = transform;
        bgmPlayer = bgmObject.AddComponent<AudioSource>();
        bgmPlayer.playOnAwake = false;                          // 게임 시작 시 재생 끄기
        bgmPlayer.loop = true;
        bgmPlayer.volume = bgmVolume;
        //bgmPlayer.clip = bgmClips;

        // 효과음 플레이어 초기화
        GameObject sfxObject = new GameObject("SFXPlayer");
        sfxObject.transform.parent = transform;
        sfxPlayers = new AudioSource[channels];

        for (int idx = 0; idx < sfxPlayers.Length; idx++)
        {
            sfxPlayers[idx] = sfxObject.AddComponent<AudioSource>();
            sfxPlayers[idx].playOnAwake = false;
            sfxPlayers[idx].volume = sfxVolume;
        }

        bgmVolume = 1.0f - PlayerPrefs.GetFloat("BGM_Volume");    // default 값이 0이기 때문에 1.0f - value로 저장
        sfxVolume = 1.0f - PlayerPrefs.GetFloat("Effect_Volume");
    }

    // BGM 사용을 위한 함수
    public void PlayBgm(BGM bgm, bool isPlay)
    {
        if (isPlay)
        {
            bgmPlayer.clip = bgmClips[(int)bgm];
            bgmPlayer.Play();
        }
        else
        {
            bgmPlayer.Stop();
        }
    }

    // 효과음 사용을 위한 함수
    public void PlaySfx(SFX sfx)
    {
        // 쉬고 있는 하나의 sfxPlayer에게 clip을 할당하고 실행
        for (int idx = 0; idx < sfxPlayers.Length; idx++)
        {
            int loopIndex = (idx + channelIndex) % sfxPlayers.Length;    // 채널 개수만큼 순회하도록 채널인덱스 변수 활용

            if (sfxPlayers[loopIndex].isPlaying) continue;               // 진행 중인 sfxPlayer는 쭉 진행

            channelIndex = loopIndex;
            sfxPlayers[loopIndex].clip = sfxClips[(int)sfx];
            sfxPlayers[loopIndex].Play();
            break;
        }
    }

    public void OnChangedBGMVolume(float value)
    {
        BgmVolume = value;
        bgmPlayer.volume = BgmVolume;
    }

    public float GetVolume(AudioType type)
    {
        return type == AudioType.BGM ? bgmPlayer.volume : sfxPlayers[0].volume;
    }

    public void OnVolumeChanged(AudioType type, float value)
    {
        PlayerPrefs.SetFloat(type == AudioType.BGM ? "BGM_Volume" : "SFX_Volume", 1.0f - value);

        if (type == AudioType.BGM)
        {
            bgmPlayer.volume = value;
        }
        else
        {
            foreach (var player in sfxPlayers)
            {
                player.volume = value;
            }
        }
    }
}
```

## SoundSettings.cs

```cs
using System.Collections;
using System.Collections.Generic;
using Unity.VisualScripting;
using UnityEngine;
using UnityEngine.Events;
using UnityEngine.UI;

public class SoundSettings : MonoBehaviour
{
    public Slider bgmSlider;
    public Slider sfxSlider;

    private AudioManager audioManager;

    private void Awake()
    {
        audioManager = AudioManager.Inst;
    }

    void Start()
    {
        if (bgmSlider != null)
        {
            bgmSlider.value = audioManager.GetVolume(AudioManager.AudioType.BGM);
            bgmSlider.onValueChanged.AddListener(value => audioManager.OnVolumeChanged(AudioManager.AudioType.BGM, value));
        }

        if (sfxSlider != null)
        {
            sfxSlider.value = audioManager.GetVolume(AudioManager.AudioType.SFX);
            sfxSlider.onValueChanged.AddListener(value => audioManager.OnVolumeChanged(AudioManager.AudioType.SFX, value));
        }
    }
}

```

