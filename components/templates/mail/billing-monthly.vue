<template>
    <div class="container" style="padding:15px;background-color:#ffffff;">
        <div style="position:relative;padding-bottom:20px;border-bottom:dashed 1px rgba(0,0,0,.1);text-align: end;">
            <img src="https://seakun.id/images/seakunid-full.png" style="height:8rem;">
        </div>
        <div style="padding-bottom:5px; padding-top: 14px;">
            <h2 style="font-size:16px;font-weight:700;color:#86d0c1;">Hai {{ fullname }},</h2>
            <p style="margin:15px 0 15px;font-size:14px;color:#787878;line-height:24px;">
                Memberitahukan kepada kamu, untuk aktivitas berlangganan <b>{{ packet }} {{ provider }}</b> 
                via <b>Seakun.id</b> sudah memasuki <b>masa pembayaran</b> untuk satu bulan kedepan. 
                <br>
                <br>
                Masa aktif layanan kamu <b>akan berakhir</b> pada <b>{{ lastActiveDate }}</b>.
                Segera lakukan pembayaran sebelum <b>{{ lastActiveDate }}</b> agar tidak 
                mengalami kendala saat menikmati layanan di <b>{{ provider }}</b>.
                <br>
            </p>
            <p style="margin:15px 0 15px;font-size:14px;color:#787878;line-height:24px;">
                Untuk berlangganan satu bulan kedepan dikenakan biaya <b>{{ price }}</b>.
                Silahkan melakukan pembayaran ke rekening berikut.
                <br>
                <br>
            </p>
            <div style="margin:15px 0 15px;font-size:14px;color:#787878;line-height:24px;text-align:center;">
                <h4 style="margin-bottom: 0px;font-size: 16px;">PT. Trivamas Tekno Solusindo</h4>
                <p style="font-weight: 600;margin: 0px;margin-bottom: 10px;">Bank Mandiri</p>
                <h4 style="margin-bottom: 0px;font-size: 16px;">No. rekening:</h4>
                <p style="font-weight: 800;font-size:18px;color:dodgerblue; margin: 0px; margin-bottom: 10px;">1150007389705</p>
                <h4 style="margin-bottom: 0px;font-size: 16px;">Nominal Pembayaran:</h4>
                <p style="font-weight: 800;font-size:18px;color:dodgerblue; margin-top: 0px;">{{ price }}</p>
                <br>
            </div>
            <p style="margin:15px 0 15px;font-size:14px;color:#787878;line-height:24px;">
                Jika telah melakukan pembayaran, silahkan kirim bukti pembayaran ke Whatsapp Seakun.id <b>085774642738</b>.
                <br>
                Akun dan Profile Netflix yang diberikan akun <b>dinonaktifkan</b> jika tidak melakukan pembayaran 
                setelah <b>{{ lastActiveDate }}</b>, terimakasih.
                <br>
                <br>
            </p>
        </div>
        <div style="padding:15px 0 30px;text-align:center;color:#787878;border-bottom:dashed 1px rgba(0,0,0,.1);">
            <h4 style="margin:20px 0 10px;">Ikuti Kami</h4>
            <a style="text-decoration:none;" href="https://www.instagram.com/seakun.id/">
            <img src="https://www.indo-iqra.com/iq_assets_web/img/ic-instagram.png" style="margin:0 5px 0;height: 30px;">
            </a>
            <a style="text-decoration:none;" href="https://www.linkedin.com/">
            <img src="https://www.indo-iqra.com/iq_assets_web/img/ic-linkedin.png" style="margin:0 5px 0;height: 30px;">
            </a>
        </div>
        <div style="padding:30px 15px 15px;text-align:center;font-size:13px;color:#aaaaaa;line-height:18px;">
            <p style="margin:15px 0 15px;font-size:12px;color:#787878;line-height:21px;">
            Punya pertanyaan? Jangan khawatir, hubungi kami <a href="mailto:admin@indoiqra.com?subject=Mau nanya dong Min" style="color:#5ac8fa;">cs@seakun.id</a>
            </p>
            <p style="margin:15px 0 15px;font-size:12px;color:#787878;line-height:21px;">
            Atau melalui Phone/WA : 085774642738
            </p>
            <p style="margin:15px 0 15px;font-size:12px;color:#787878;line-height:21px;">
            Harap jangan membalas e-mail ini, karena e-mail ini dikirimkan secara otomatis oleh sistem.
            </p>
            <p style="margin:15px 0 15px;font-size:12px;color:#787878;line-height:21px;">
            Copyright 2020 © Seakun.id. All Rights Reserved. 
            </p>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        fullname: String,
        packet: String,
        provider: String,
        price: String,
        lastActiveDate: String,
    }
}
</script>

<style lang="scss" scoped>
body {
    margin:0;
    background-color:#fafafa;
    font-family:Nunito,sans-serif;
}
.container {
    margin:0;
    background-color:#fafafa;
    font-family:Nunito,sans-serif;
    margin: 30px auto;
    width:100%;
    max-width: 600px;
}
@media screen and (max-width: 768px) {
    .container {
        margin: 15px auto;
        width: calc(100% - 60px);
        max-width: 100%;
    }
}
</style>