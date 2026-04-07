This supplementary material provides additional information to the manuscript, including deepfake generation pipeline, manipulated examples, experimental details, and deepfake methods.

## Deepfake Image Generation
### Single-Face Scene Manipulation:
As shown in Fig. 1(a), 
(1) Face Parsing Stage: We utilize the dlib model to extract facial landmark points and generate random facial region masks based on these landmarks. 
(2) Face Generation Stage: Face images and their corresponding masks are fed into a visual generative model, which synthesizes locally tampered images under the guidance of either the masks or textual prompts (required by certain models). We employ DeepSeek-V3 to generate the relevant textual prompts. In this study, we focus on models operating within the Facial Editing (FE) paradigm, as detailed in Table below.

### Multi-face Scene Manipulation:
As depicted in Fig. 1(b),
(1) Face Preprocessing Stage: MTCNN [zhang2016joint] is employed for single-face detection, followed by mask segmentation of each detected face region using a U-Net model.
(2) Face Generation Stage: Randomly selected single-face images are input into the visual generation model to obtain forged images. Similarly, we employ DeepSeek-V3 to generate textual prompts for generative models that require text conditioning.
(3) Face Fusion Stage: The forged faces and their masks are jointly fed into a fusion model with the original multi-face image to generate locally forged multi-face images. 
The fusion method integrates a handcrafted Poisson blending [perez2023poisson] method and a diffusion-based fusion model [yu2024facechain].
The generation models in this scenario span four forgery modes: Face Swapping (FS), Full-Face Synthesis (FFS), Face Editing (FE), and Face Reenactment (FR).
In addition, benefiting from the decoupled nature of the multi-face forgery process, we introduce an Hybrid Facial Forgery (HFF) Mode (as shown in Figure below), wherein different deepfake techniques are applied to different faces within the same multi-face image.

figures/generation_pipeline_img_1.pdf
<div align="center">
  <img src="https://github.com/inclusionConf/DDL/blob/main/">
</div>
Fig. Image generation pipeline. The red boxes denote forged images. Italic text indicates generation processing operations. The Generation Model refers to different visual generation models. The Face Blending denotes the fusion model for integrating the forged face with the real background. The LLM synthesizes face-related prompts.

## Deepfake Audio-Video Generation
### Audio-Visual Local Manipulation:
As shown in Fig. 2(a),
(1) Audio Preprocessing Stage: We use FFmpeg to separate video and audio streams, apply a Denoiser [defossez2020real] to extract clean speech and background noise components, and employ Whisper [radford2023robust] for speech recognition to obtain textual transcripts of the human voice. Subsequently, we employ DeepSeek-V3 to perform local text manipulation, including replacement, deletion, and insertion operations.
(2) Audio Forgery Stage: The manipulated text is then fed into an audio generation model to synthesize forged audio, whose length is normalized before being merged with the background noise to form the final forged audio sample.
(3) Video Forgery Stage: We employ audio-driven face generation models to synthesize forged face videos conditioned on fake audio. Furthermore, to diversify the types of local audio-visual forgery, we incorporate non-audio-driven video generation models. In this setting, we adopt a disentangled pipeline: original audio and video streams are first separated, then processed by independent audio and video forgery models, respectively, before being recombined into a final forged sequence.
(4) Audio-Visual Merging Stage: According to replacement, deletion, and insertion editing strategies, the forged audio and forged video are combined to create three types of forged audio-visual content: Fake Audio and Fake Visual, Fake Audio and Real Visual, and Real Audio and Fake Visual.
Additionally, we introduce an Audio-Visual Asynchronous Manipulation (AVAM) mode (as shown in Figure below) in which forged audio and video are not synchronized, thereby further mimicking real-world scenarios.

### Audio-Visual Full Synthesis:
As shown in Fig. 2(b),
(1) Prompt Generation Stage: We employ DeepSeek-V3 to generate face-related prompts, such as textual descriptions of expressions and actions like speaking, singing, and laughing.
(2) Video Synthesis Stage: The generated prompt text is input into a Text2Video generation model to obtain the corresponding video content; alternatively, the prompt text, along with a facial image, is provided to an Image2Video model to generate a fully synthesized video featuring the given face.
(3) Audio Synthesis Stage: The synthesized video is then fed into a Video2Audio generation model to produce the corresponding audio track.
(4) Audio-Visual Merging Stage: The resulting facial video and synthesized audio are combined to form a complete, fully synthesized audio-visual sample.
Furthermore, end-to-end audio-visual synthesis models, such as Kling 2.1 [kling2025] and Wan 2.1 [wan2025], are also employed to further improve synthesis quality. This novel forgery paradigm differs significantly from traditional audio-driven or video-driven deepfake techniques, substantially reducing artifacts that may be introduced by multi-frame compositing.

figures/generation_pipeline_av_1.pdf
<div align="center">
  <img src="https://github.com/inclusionConf/DDL/blob/main/">
</div>
Fig. Audio-Visual generation pipeline. The LLM manipulates speech transcripts (a) or synthesizes face prompts (b). Generation Model denotes visual/audio generation models. Red waveforms/faces indicate manipulated audio/video. <Replace/Delete/Insert> in green boxes denote local manipulations. Italicized Whisper/Denoiser denote transcript generation/noise separation; other italics indicate preprocessing. Dashed lines represent optional inputs.

## Manipulated Examples
We primarily demonstrate examples of three innovative forgery patterns and different blending methods in image datasets.

### Forgery Mode Examples
In this section, we present innovative forgery modes as follows:

Hybrid Facial Forgery (HFF) Mode: As illustrated in Fig. below, compared to conventional forgery mode, HFF applies two distinct forgery techniques (A and B) to manipulate different faces within a multi-person image. This approach increases the complexity of forgery content, reflecting the diverse forgery landscape in real-world scenarios.

Audio-Visual Asynchronous Manipulation (AVAM) Mode: As shown in Fig. below, AVAM selects different temporal segments to modify audio content and video facial regions separately. This method also aligns with the multifaceted nature of real-world forgeries. Moreover, in audio-visual modalities, we incorporate the HFF forgery pattern by applying varied manipulation techniques across different temporal segments.

Audio-Visual Full Synthesis (AVFS) Mode: As presented in Fig. below, AVFS generates complete audio-visual content using text or single images. In the era of rapidly advancing artificial intelligence and generative technologies, such forgery patterns are expected to proliferate in real-world contexts.

### Blending Method Examples
As illustrated in Fig. below, (a) depicts Poisson fusion, a method commonly employed in previous research. (b) represents the state-of-the-art diffusion-based fusion technique, which, with the advancement of Artificial Intelligence and Generative Computing (AIGC) technologies, gradually emerges as the predominant fusion approach in real-world scenarios. Our DDL deliberately adopts this method to simulate contemporary trends and challenges in image manipulation.

figures/supp_forgery_mode_1.pdf

Fig. Examples of Hybrid Facial Forgery (HFF) Mode.

figures/supp_forgery_mode_2.pdf

Fig. Examples of Audio-Visual Asynchronous Manipulation (AVAM) Mode. The specific audio-visual sample demonstrations are stored in Example_AVAM_Mode folder within the same supplementary material archive.

figures/supp_forgery_mode_3.pdf

Fig. Examples of Audio-Visual Full Synthesis (AVFS) Mode. The specific audio-visual sample demonstrations are stored in Example_AVFS_Mode folder within the same supplementary material archive.

figures/supp_forgery_mode_4.pdf

Fig. Examples of Blending Methods.

## Experimental Details
In this section, we provide detailed descriptions of the baseline models, training procedures, and the use of unseen test sets.

### Baseline Models
Image task: We select the face forgery localization method MSCCNet [miao2023multi] and the conventional image manipulation localization models SparseViT [su2025can] and Mesorch [zhu2025mesoscopic]. MSCCNet [miao2023multi] has a classification head that can simultaneously output image classification and forgery localization results, while SparseViT [su2025can] and Mesorch [zhu2025mesoscopic] lack the classification head and can only output forgery localization results. In this study, we take the average of the forgery masks predicted by SparseViT [su2025can] and Mesorch [zhu2025mesoscopic] as the image classification results, following the setting of MVSS-Net [chen2021image].

Audio-Visual task: We choose audio-visual localization models BA-TFD [cai2022you], BA-TFD+ [cai2022you], and UMMAFormer [zhang2023ummaformer], all of which lack classification heads. We use the maximum score of the predicted proposals as the image classification result.

### Training Details
We refer to the open-source versions of the aforementioned models.
