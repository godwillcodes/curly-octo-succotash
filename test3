<?php
   /**
    *
    * Template Name: Search Results - Medical
    *
    * The template for displaying content from pagebuilder.
    *
    * This is the template that displays pages without title in fullwidth layout. Suitable for use with Pagebuilder.
    *
    * @link https://codex.wordpress.org/Template_Hierarchy
    *
    * @package PesaBazaar
    */
   
   get_header();
   get_template_part( 'template-parts/medical/search-banner');
   get_template_part( 'template-parts/medical/search-radio');
   get_template_part( 'template-parts/medical/search-results');
   get_template_part( 'template-parts/cta');
   get_template_part( 'template-parts/secondary-footer');
   get_footer();
   ?>
<script>
   var app = new Vue({
    el: "#page",
    data: {
        grouping: null,
        models: [],
        comparisondata: [],
        motorbenefits: [],
        motorexcesses: [],
        motorimportants: [],
   free_benefits: [],
   additional_benefits: [],
        step: 1,
        selectedCompanies: '',
		sortOrder: 'asc', // Default sorting order
   selectedOptions: [],
        submitting: true,
        lead: null,
        showBenefits: false,
   showMedicalDiv: false,
        showMoreDetails: false,
   showOptionalBenefits: false,
   totalOutpatient: 0,
		showFeatures: false,
        detailsVisible: true,
		selectedResult: null,
		grandtotaldisplay: null,
		addedOutPatient: null,
		fullSelectedBenefits: null,
		emaildata: {
			action: "success",
			id: null,
			data: null,
		},
        res: {
            action: "getMedicalResults",
            id: null,
        },
    },
    created() {
        this.showBenefits = false;
        this.showMoreDetails = false; // Set showContent to false initially
    },
   computed: {
	   total() {
	   return this.selectedOptions.reduce((acc, cur) => acc + Number(cur), 0);
	   },
   },
    methods: {
		toggleFeatureList() {
      this.showFeatures = !this.showFeatures;
    },
		 sortByPrice(order) {
      this.sortOrder = order;
    },
   ageCalculator(ageBrackets, dateOfBirth){
     var currentDate = new Date();
     var birthDate = new Date(dateOfBirth);
     var age = currentDate.getFullYear() - birthDate.getFullYear();
     var finalPrincipal = 0;
   
     // Loop through the age brackets
     for (var i = 0; i < ageBrackets.length; i++) {
		var bracket = ageBrackets[i];
		if (age <= parseInt(bracket.age_bracket)) {
		  // Return the principal based on the age bracket
		  return bracket.principal;
   		}
     }
   return finalPrincipal;

   },
	ageCalculatorWithTax(ageBrackets, dateOfBirth, tax_rate, stamp_duty){
     var currentDate = new Date();
     var birthDate = new Date(dateOfBirth);
     var age = currentDate.getFullYear() - birthDate.getFullYear();
     var finalPrincipal = 0;
   
     // Loop through the age brackets
     for (var i = 0; i < ageBrackets.length; i++) {
		var bracket = ageBrackets[i];
		if (age <= parseInt(bracket.age_bracket)) {
		  // Return the principal based on the age bracket
		  
		  var tax = parseFloat(bracket.principal)*parseFloat(tax_rate)/100;
// 		  console.log(tax_rate);
// 		  console.log(bracket.principal);
// 		  console.log(tax);
		  return (tax + parseFloat(bracket.principal) + parseFloat(stamp_duty));
   		}
     }
   return finalPrincipal;
		

   },
   ageOutpatientCalculator(ageBrackets, dateOfBirth){
     var currentDate = new Date();
     var birthDate = new Date(dateOfBirth);
     var age = currentDate.getFullYear() - birthDate.getFullYear();
     var finalPrincipal = [];
   
     // Loop through the age brackets
     for (var i = 0; i < ageBrackets.length; i++) {
		var bracket = ageBrackets[i];
		if (age >= parseInt(bracket.age_bracket_minimum) && age <= parseInt(bracket.age_bracket_maximum)) {
		  // Return the principal based on the age bracket
		  finalPrincipal.push(ageBrackets[i]);
   		}
     }
   return finalPrincipal;
	console.log(finalPrincipal);
	},
		
   formattedCoverLimit() {
   return this.formatCurrency(this.lead.cover_limit);
   },
   formatCurrency(value) {
   var num_parts = parseFloat(value).toFixed(0).toString().split(".");
    num_parts[0] = num_parts[0].replace(/\B(?=(\d{3})+(?!\d))/g, ",");
    return num_parts.join(".");
   },
	formatPercentage(value) {
		return value + "%";
	},
   outpatientCover(val, data, fulldata) {
	   this.addedOutPatient = val;
	   console.log("Added OutPatient:", this.addedOutPatient);
		this.showOptionalBenefits = true;
		this.totalOutpatient = val;
		var container = document.getElementById('666' + data.id);
		var nonTaxcontainer = document.getElementById('444' + data.id).value;
		var arr = nonTaxcontainer.split(',');
		var rate = arr.join('');

        var grandTotal = 0;
		var grand = parseFloat(val) + parseFloat(rate) + parseFloat(fulldata.stamp_duty);
		grandTotal = ((parseFloat(fulldata.tax_rate) / 100) * grand) + grand;
        container.innerHTML = this.formatCurrency(grandTotal);
	   
	   this.grandtotaldisplay = grandTotal;
	   
   },
		removeOutpatientCover(data) {
			this.addedOutPatient = null;
			this.totalOutpatient = null;
			this.showOptionalBenefits = false;

			var container = document.getElementById('666' + data.id);
			var fallBackContainer = document.getElementById('555' + data.id).value;
			var arrFallBac = fallBackContainer.split(',');
			var rateFine = arrFallBac.join('');
			// Update the value of 'container' to use 'fallBackContainer'
			container.innerHTML = this.formatCurrency(rateFine);
			
			const existingData = localStorage.getItem("myBazaarCartMedical_" + data.id);
			
			if (undefined !== existingData && existingData != null) {
				var res = JSON.parse(existingData);
				
				var data1 = res.data;
				var total1 = res.total;
				var benefit1 = [];
				var client1 = res.client;
				
				const selectedBenefits = {
					data: data1,
					total: total1,
					benefit: benefit1,
					client: client1
				};
				
				localStorage.setItem("myBazaarCartMedical_" + data.id, JSON.stringify(selectedBenefits));
			}
			
			this.fullSelectedBenefits = null;
			this.grandtotaldisplay = rateFine;
		},


   selectedData(val, data, client, fulldata) {
    var container = document.getElementById('666' + data.id);
    var nonTaxcontainer = document.getElementById('444' + data.id).value;
    var fallBackContainer = document.getElementById('555' + data.id).value;
    var arr = nonTaxcontainer.split(',');
    var rate = arr.join('');
//    console.log('Total Outpatient:', this.totalOutpatient);
    var arrFallBac = fallBackContainer.split(',');
    var rateFine = arrFallBac.join('');
    var totalOutpatient = parseFloat(this.totalOutpatient);
   
//     console.log("data:", data);
//     console.log("client:", client);
//     console.log("container:", container);
//     console.log("nonTaxcontainer:", nonTaxcontainer);
//     console.log("fallBackContainer:", fallBackContainer);
//     console.log("arr:", arr);
//     console.log("rate:", rate);
//     console.log("arrFallBac:", arrFallBac);
//     console.log("rateFine:", rateFine);
   
    const existingData = localStorage.getItem("myBazaarCartMedical_" + data.id); // Unique storage key
   
//     console.log("existingData:", existingData);
   
    if (undefined !== existingData && existingData != null) {
        var res = JSON.parse(existingData);
   
        if (res.data.id == data.id) {
            var storageBenefit = res.benefit;
   
            var filteredResult = storageBenefit.filter((br) => {
                return (parseFloat(br.cost) == parseFloat(val.cost)) && (br.content == val.content);
            });
   
            if (filteredResult.length != 0 && filteredResult[0] != null && filteredResult[0] != undefined) {
   
                var removingFilter = storageBenefit.filter((rpt) => {
                    return (rpt.content != filteredResult[0].content);
                });
   
                storageBenefit = removingFilter;
   
                var benefitsTotal = 0;
                var fullTotal = 0;
                var totalTax = 0;
                var grandTotal = 0;
                var nontaxbnefit = 0;
   
                storageBenefit.forEach(element => {
                    if (parseFloat(element.rate) <= 0) {
                        nontaxbnefit = nontaxbnefit + parseFloat(element.cost);
                    } else {
                        benefitsTotal = benefitsTotal + parseFloat(element.cost);
                    }
                });
   
                fullTotal = nontaxbnefit + parseFloat(rate) + parseFloat(fulldata.stamp_duty) + totalOutpatient;
   
                grandTotal = ((parseFloat(fulldata.tax_rate) / 100) * fullTotal) + fullTotal;
				
				this.grandtotaldisplay = grandTotal;
				
				this.fullSelectedBenefits = storageBenefit;
   
                container.innerHTML = this.formatCurrency(grandTotal);
   
            } else {
                storageBenefit.push(val);
   
                var benefitsTotal = 0;
                var fullTotal = 0;
                var totalTax = 0;
                var grandTotal = 0;
                var nontaxbnefit = 0;
   
                storageBenefit.forEach(element => {
                    if (parseFloat(element.rate) <= 0) {
                        nontaxbnefit = nontaxbnefit + parseFloat(element.cost);
                    } else {
                        benefitsTotal = benefitsTotal + parseFloat(element.cost);
                    }
                });
   
                fullTotal = nontaxbnefit + parseFloat(rate) + parseFloat(fulldata.stamp_duty) + totalOutpatient;
   
                grandTotal = ((parseFloat(fulldata.tax_rate) / 100) * fullTotal) + fullTotal;
				
				this.grandtotaldisplay = grandTotal;
				
				this.fullSelectedBenefits = storageBenefit;
   
                container.innerHTML = this.formatCurrency(grandTotal);
            }
   
            const selectedBenefits = {
                data: data,
                total: grandTotal,
                benefit: storageBenefit,
                client: client
            };
   
            localStorage.setItem("myBazaarCartMedical_" + data.id, JSON.stringify(selectedBenefits));
   
        } else {
            var benefitsTotal = 0;
            var fullTotal = 0;
            var totalTax = 0;
            var grandTotal = 0;
            var nontaxbnefit = 0;
   
            if (parseFloat(val.rate) <= 0) {
                var grandT = parseFloat(val.cost) + parseFloat(rateFine) + parseFloat(fulldata.stamp_duty) + totalOutpatient;
				
				grandTotal = ((parseFloat(fulldata.tax_rate) / 100) * grandT) + grandT;
            } else {
                benefitsTotal = parseFloat(val.cost) + parseFloat(rate) + totalOutpatient;
   
                totalTax = (parseFloat(fulldata.tax_rate) / 100) * benefitsTotal;
   
                grandTotal = benefitsTotal + totalTax;
            }
			
			this.grandtotaldisplay = grandTotal;
   
            container.innerHTML = this.formatCurrency(grandTotal);
   
            var benefitdata = [val];
   
            const selectedBenefits = {
                data: data,
                total: grandTotal,
                benefit: benefitdata,
                client: client
            };
			
			this.fullSelectedBenefits = selectedBenefits;
   
            localStorage.setItem("myBazaarCartMedical_" + data.id, JSON.stringify(selectedBenefits)); // Unique storage key
        }
   
    } else {
        var benefitsTotal = 0;
        var fullTotal = 0;
        var totalTax = 0;
        var grandTotal = 0;
        var nontaxbnefit = 0;
   
        if (parseFloat(val.rate) <= 0) {
            var grand = parseFloat(val.cost) + parseFloat(rate) + parseFloat(fulldata.stamp_duty) + totalOutpatient;
			
			grandTotal = ((parseFloat(fulldata.tax_rate) / 100) * grand) + grand;
			
        } else {
            benefitsTotal = parseFloat(val.cost) + parseFloat(rate) + totalOutpatient;
   
            totalTax = (parseFloat(val.rate) / 100) * benefitsTotal;
   
            grandTotal = benefitsTotal + totalTax;
        }
		
		this.grandtotaldisplay = grandTotal;
   
        container.innerHTML = this.formatCurrency(grandTotal);
   
        var benefitdata = [val];
   
        const selectedBenefits = {
            data: data,
            total: grandTotal,
            benefit: benefitdata,
            client: client
        };
		
		this.fullSelectedBenefits = benefitdata;
   
        localStorage.setItem("myBazaarCartMedical_" + data.id, JSON.stringify(selectedBenefits)); // Unique storage key
	}
   },
   
	   redirectitosuccesspage(id){
		const existingData = localStorage.getItem("myBazaarCartMedical_"+id);

		if (undefined !== existingData && existingData != null) {
			var res = JSON.parse(existingData);
			this.emaildata.id = id;
			this.emaildata.data = res.client;
			
			if (res.data.id == id) {
				var storageClient = res.client;
				var postdata = this.emaildata;
				const formData = new FormData();
				for (var key in postdata) {
					formData.append(key, postdata[key]);
				}
				try {
					fetch("<?php echo admin_url('admin-ajax.php'); ?>", {
						method: "POST",
						body: formData,
					}).then((res) => {
						return res.json();
					}).then((data) => {
// 						this.comparisondata = data.data;
// 						this.lead = data.clientdata;
						if(data != null && data.code === 422 ){
// 							this.errors = data;
							console.log(data);
							// console.log(data[0].phone)
						}else{
							let url = '<?php echo site_url();?>/thank-you/?product=medical';
							window.location.replace(url);
// 							this.applynow = false
// 							this.successModal = true
// 							let url = new URL(window.location.href);
// 							url += '&state=success'
						}
					}).catch((err) => console.error(err));
					this.submitting = false;
				} catch (e) {
					console.log("error", e);
					this.submitting = false;
					return;
				}
			}
			
			let url = '<?php echo site_url();?>/thank-you/?product=medical';
			window.location.replace(url);
		}else{
			let url = '<?php echo site_url();?>/thank-you/?product=medical';
			window.location.replace(url);
		}
	   },
        async getresults(id) {
            console.log(id);
            this.res.id = id;
            var postdata = this.res;
            const formData = new FormData();
            for (var key in postdata) {
                formData.append(key, postdata[key]);
            }
            this.submitting = true;
            try {
                console.log(formData);
                fetch("<?php echo admin_url('admin-ajax.php'); ?>", {
                    method: "POST",
                    body: formData,
                })
                    .then((res) => {
                        return res.json();
                    })
                    .then((data) => {
                        this.comparisondata = data;
                    })
                    .catch((err) => console.error(err));
   
                this.submitting = false;
            } catch (e) {
                console.log("error", e);
   
                this.submitting = false;
                return;
            }
        },
        toggleBenefits(data) {
			this.grandtotaldisplay = document.getElementById('666'+data).innerHTML;
			
			this.selectedResult = data;
            this.showBenefits = !this.showBenefits;
            this.showMoreDetails = false;
			const button = document.getElementById('benefits-btn' + data);
		  if (button) {
			// Hide the button by setting display to 'none'
			button.style.display = 'none';
		  }
        },
        toggleDetails() {
            this.showMoreDetails = !this.showMoreDetails;
            this.detailsVisible = false;
			this.showBenefits = false;
        },
        hideDetails() {
            this.showMoreDetails = false;
            this.detailsVisible = true;
        },
      
    },
    mounted() {
		localStorage.clear();
        let uri = window.location.href.split("?");
        if (uri.length >= 2) {
            let vars = uri[1].split("&");
            let getVars = {};
            let tmp = "";
            vars.forEach(function (v) {
                tmp = v.split("=");
                if (tmp.length == 2) getVars[tmp[0]] = tmp[1];
            });
            this.res.id = getVars.id;
            //                     if (getVars.id != null && getVars.id == '') {
            //                         this.getresults(getVars.id);
   
            var postdata = this.res;
            const formData = new FormData();
            for (var key in postdata) {
                formData.append(key, postdata[key]);
            }
            this.submitting = true;
            try {
                fetch("<?php echo admin_url('admin-ajax.php'); ?>", {
                    method: "POST",
                    body: formData,
                })
                    .then((res) => {
                        return res.json();
                    })
                    .then((data) => {
                        console.log(data);
                        this.comparisondata = data.data;
                        this.lead = data.clientdata;
                    })
                    .catch((err) => console.error(err));
   
                this.submitting = false;
            } catch (e) {
                console.log("error", e);
   
                this.submitting = false;
                return;
            }
            //                     }
        } else {
            var url = "https://pesabazaar.belva.co.ke/api/setup";
            fetch(url)
                .then((response) => {
                    return response.json();
                })
                .then((data) => {
                    this.grouping = data;
                    this.models = data.brands;
                    this.use = data.motortypes;
                });
        }
    },
   });
   
</script>
